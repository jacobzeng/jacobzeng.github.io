---
layout: post
title:  "Encrypted Media Extensions"
date:   2015-05-07 17:00:00
categories: eme
tags: eme video w3c encryption
---

EME提供了一套API可以让web applications和content protection systems交互，允许播放加密后的audio和video.

EME理论上来说是可以让同一个app和加密的文件能够在所有的browser中播放，无论是基于什么样的content protection system. 这得依靠标准的API和流程，以及Common Encryption.

---

要实现和支持EME中涉及到的各组件
===

+ **Key System:** a content protection(DRM) mechanism. EME doesn't define Key Systems themselves, apart from Clear Key.

+ **Content Decryption Module(CDM):** a client-side software or hardware mechanism that enables playback of encrypted media. As with Key Systems, EME doesn't define any CDMs, but provides an interface for applications to interact with CDMs that are avaliable.

+ **License(Key) server:** interacts with a CDM to provide keys to decrypt media. Negotiation with license server is the resposibility of the application.

+ **Packaging service:** encodes and encrypts media for distribution/consumption.

---

How does EME work?
===

>If multiple formats or codecs are available, __MediaSource.isTypeSupported()__ or __HTMLMediaElement.canPlayType()__ can both be used to select the right one. However, the CDM may only support a subset of what the browser supports for unencrypted content. It's best to negotiate a __MediaKeys__ configuration before selecting a format and codec. If the application waits for the encrypted event but then __MediaKeys__ shows it can't handle the chosen format/codec, it may be too late to switch without interrupting playback.
>    
>The recommended flow is to negotiate MediaKeys first, using __MediaKeysSystemAccess.getConfiguration()__ to find out the negotiated configuration.
>    
>If there is only one format/codec to choose from, then there's no need for __getConfiguration()__. However, it's still preferable to set up MediaKeys first. The only reason to wait for the encrypted event is if there is no way of knowing whether the content is encrypted or not, but in practice that's unlikely.

---

1. A web application attempts to play audio or video that has one or more encrypted streams.

2. The browser recognizes that the media is encrypted (see box below for how that happens) and fires an encrypted event with metadata (initData) obtained from the media about the encryption.

3. The application handles the encrypted event:
   1.   If no MediaKeys object has been associated with the media element, first select an available Key System by using __navigator.requestMediaKeySystemAccess()__ to check what Key Systems are available, then create a MediaKeys object for an available Key System via a MediaKeySystemAccess object. Note that initialization of the MediaKeys object should happen before the first encrypted event. Getting a license server URL is done by the app independently of selecting an available key system. A MediaKeys object represents all the keys available to decrypt the media for an audio or video element. It represents a CDM instance and provides access to the CDM, specifically for creating key sessions, which are used to obtain keys from a license server.
   2.   Once the MediaKeys object has been created, assign it to the media element: setMediaKeys() associates the MediaKeys object with an HTMLMediaElement, so that its keys can be used during playback, i.e. during decoding.

4. The app creates a MediaKeySession by calling createSession() on the MediaKeys. This creates a MediaKeySession, which represents the lifetime of a license and its key(s).

5. The app generates a license request by passing the media data obtained in the encrypted handler to the CDM, by calling generateRequest() on the MediaKeySession.

6. The CDM fires a message event: a request to acquire a key from a license server.

7. The MediaKeySession object receives the message event and the application sends a message to the license server (via XHR, for example).

8. The application receives a response from the license server and passes the data to the CDM using the update() method of the MediaKeySession.

9. The CDM decrypts the media using the keys in the license. A valid key may be used, from any session within the MediaKeys associated with the media element. The CDM will access the key and policy, indexed by Key ID.

10. Media playback resumes.

---

>How does the browser know that media is encrypted?
>    
>This information is in the metadata of the media container file, which will be in a format such as [ISO BMFF](https://en.wikipedia.org/wiki/ISO_base_media_file_format) or [WebM](https://en.wikipedia.org/wiki/WebM). For ISO BMFF this means header metadata, called the protection scheme information box. WebM uses the Matroska ContentEncryption element, with some WebM-specific additions. Guidelines are provided for each container in an EME-specific registry.

---

Note that there may be multiple messages between the CDM and the license server, and all communication in this process is opaque to the browser and application: messages are only understood by the CDM and license server, although the app layer can see what type of message the CDM is sending. The license request contains proof of the CDM's validity (and trust relationship) as well as a key to use when encrypting the content key(s) in the resulting license.

What do CDMs actually do?
===

An EME implementation does not in itself provide a way to decrypt media: it simply provides an API for a web application to interact with Content Decryption Modules.

What CDMs actually do is not defined by the EME spec, and a CDM may handle decoding (decompression) of media as well as decryption. From least to most robust, there are several potential options for CDM functionality:

+ Decryption only, enabling playback using the normal media pipeline, for example via a `<video>` element.
+ Decryption and decoding, passing video frames to the browser for rendering.
+ Decryption and decoding, rendering directly in the hardware (for example, the GPU).

There are multiple ways to make a CDM available to a web app:

+ Bundle a CDM with the browser.
+ Distribute a CDM separately.
+ Build a CDM into the operating system.
+ Include a CDM in firmware.
+ Embed a CDM in hardware.

How a CDM is made available is not defined by the EME spec, but in all cases the browser is responsible for vetting and exposing the CDM.

EME doesn't mandate a particular Key System; among current desktop and mobile browsers, Chrome supports Widevine and IE11 supports PlayReady.

Getting a key from a license server
===

In typical commercial use, content will be encrypted and encoded using a packaging service or tool. Once the encrypted media is made available online, a web client can obtain a key (contained within a license) from a license server and use the key to enable decryption and playback of the content.

The following code (adapted from the [spec examples](http://w3c.github.io/encrypted-media/#examples)) shows how an application can select an appropriate key system and obtain a key from a license server.

{% highlight javascript %}
var video = document.querySelector('video');

var config = [{initDataTypes: ['webm'],
  videoCapabilities: [{contentType: 'video/webm; codecs="vp9"'}]}];

if (!video.mediaKeys) {
  navigator.requestMediaKeySystemAccess('org.w3.clearkey',
      config).then(
    function(keySystemAccess) {
      var promise = keySystemAccess.createMediaKeys();
      promise.catch(
        console.error.bind(console, 'Unable to create MediaKeys')
      );
      promise.then(
        function(createdMediaKeys) {
          return video.setMediaKeys(createdMediaKeys);
        }
      ).catch(
        console.error.bind(console, 'Unable to set MediaKeys')
      );
      promise.then(
        function(createdMediaKeys) {
          var initData = new Uint8Array([...]);
          var keySession = createdMediaKeys.createSession();
          keySession.addEventListener('message', handleMessage,
              false);
          return keySession.generateRequest('webm', initData);
        }
      ).catch(
        console.error.bind(console,
          'Unable to create or initialize key session')
      );
    }
  );
}

function handleMessage(event) {
  var keySession = event.target;
  var license = new Uint8Array([...]);
  keySession.update(license).catch(
    console.error.bind(console, 'update() failed')
  );
}
{% endhighlight %}

Common Encryption
===

Common Encryption solutions allow content providers to encrypt and package their content once per container/codec and use it with a variety of Key Systems, CDMs and clients: that is, any CDM that supports Common Encryption. For example, a video packaged using Playready could be played back in a browser using a Widevine CDM obtaining a key from a Widevine license server.

This is in contrast to legacy solutions that would only work with a complete vertical stack, including a single client that often also included an application runtime.

[Common Encryption](http://www.iso.org/iso/iso_catalogue/catalogue_tc/catalogue_detail.htm?csnumber=60397) (CENC) is an ISO standard defining a protection scheme for ISO BMFF; a similar concept applies to WebM.

Clear Key
===

Although EME does not define DRM functionality, the spec currently mandates that all browsers supporting EME must implement Clear Key. Using this system, media can be encrypted with a key and then played back simply by providing that key. Clear Key can be built into the browser: it does not require the use of a separate decryption module.

While not likely to be used for many types of commercial content, Clear Key is fully interoperable across all browsers that support EME. It is also handy for testing EME implementations, and applications using EME, without the need to request a content key from a license server. There is a simple Clear Key example at [simpl.info/ck](http://simpl.info/eme/clearkey). Below is a walkthrough of the code, which parallels the steps described above, though without license server interaction.

{% highlight javascript %}
// Define a key: hardcoded in this example
// – this corresponds to the key used for encryption
var KEY = new Uint8Array([
  0xeb, 0xdd, 0x62, 0xf1, 0x68, 0x14, 0xd2, 0x7b,
  0x68, 0xef, 0x12, 0x2a, 0xfc, 0xe4, 0xae, 0x3c
]);

var config = [{
  initDataTypes: ['webm'],
  videoCapabilities: [{
    contentType: 'video/webm; codecs="vp8"'
  }]
}];

var video = document.querySelector('video');
video.addEventListener('encrypted', handleEncrypted, false);

navigator.requestMediaKeySystemAccess('org.w3.clearkey', config).then(
  function(keySystemAccess) {
    return keySystemAccess.createMediaKeys();
  }
).then(
  function(createdMediaKeys) {
    return video.setMediaKeys(createdMediaKeys);
  }
).catch(
  function(error) {
    console.error('Failed to set up MediaKeys', error);
  }
);

function handleEncrypted(event) {
  var session = video.mediaKeys.createSession();
  session.addEventListener('message', handleMessage, false);
  session.generateRequest(event.initDataType, event.initData).catch(
    function(error) {
      console.error('Failed to generate a license request', error);
    }
  );
}

function handleMessage(event) {
  // If you had a license server, you would make an asynchronous XMLHttpRequest
  // with event.message as the body.  The response from the server, as a
  // Uint8Array, would then be passed to session.update().
  // Instead, we will generate the license synchronously on the client, using
  // the hard-coded KEY at the top.
  var license = generateLicense(event.message);

  var session = event.target;
  session.update(license).catch(
    function(error) {
      console.error('Failed to update the session', error);
    }
  );
}

// Convert Uint8Array into base64 using base64url alphabet, without padding.
function toBase64(u8arr) {
  return btoa(String.fromCharCode.apply(null, u8arr)).
      replace(/\+/g, '-').replace(/\//g, '_').replace(/=*$/, '');
}

// This takes the place of a license server.
// kids is an array of base64-encoded key IDs
// keys is an array of base64-encoded keys
function generateLicense(message) {
  // Parse the clearkey license request.
  var request = JSON.parse(new TextDecoder().decode(message));
  // We only know one key, so there should only be one key ID.
  // A real license server could easily serve multiple keys.
  console.assert(request.kids.length === 1);

  var keyObj = {
    kty: 'oct',
    alg: 'A128KW',
    kid: request.kids[0],
    k: toBase64(KEY)
  };
  return new TextEncoder().encode(JSON.stringify({
    keys: [keyObj]
  }));
}
{% endhighlight %}

To test this code, you need an encrypted video to play. Encrypting a video for use with Clear Key can be done for WebM as per the [webm_crypt instructions](https://docs.google.com/document/d/17d6_KX5jX0gY1ygYbjqOEdVzuUGkPO53wL8t40dMGeQ/edit?usp=sharing). Commercial services are also available (for ISO BMFF/MP4 at least) and other solutions are being developed.

---

Chrome/Widevine CDM
===

![5]({{site.baseUrl}}/assets/Encrypted Media Extensions/5.png)


Further reading
===

Specs & standards
---

+ [EME spec](https://w3c.github.io/encrypted-media/) : latest Editor's Draft
+ [Common Encryption (CENC)](http://www.iso.org/iso/iso_catalogue/catalogue_tc/catalogue_detail.htm?csnumber=60397)
+ [Media Source Extensions](http://www.w3.org/TR/media-source/)
+ [DASH standard](http://standards.iso.org/ittf/PubliclyAvailableStandards/c057623_ISO_IEC_23009-1_2012.zip)
+ [Overview of the DASH standard](http://dashif.org/mpeg-dash/)

Articles
---

+ [DTG Webinar](http://vimeo.com/62269279) (partially obsolete)
+ [What is EME?](https://hsivonen.fi/eme/)
+ [HTML5 Rocks Media Source Extensions article](http://updates.html5rocks.com/2011/11/Stream-video-using-the-MediaSource-API)
+ MPEG-DASH Test Streams: [BBC R&D blog post](http://bbc.co.uk/rd/blog/2013/09/mpeg-dash-test-streams)

Demos
---

+ Clear Key demo: [simpl.info/ck](http://simpl.info/eme/clearkey)
+ [Media Source Extensions (MSE) demo](http://simpl.info/mse)
+ Google's [Shaka Player](https://github.com/google/shaka-player) implements a DASH client with EME support