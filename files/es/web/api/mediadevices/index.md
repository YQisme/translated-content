---
title: MediaDevices
slug: Web/API/MediaDevices
---

{{APIRef("WebRTC")}}{{SeeCompatTable}}

The **`MediaDevices`** interface provides access to connected media input devices like cameras and microphones, as well as screensharing.

## Properties

_None._

## Methods

- {{ domxref("MediaDevices.getUserMedia()") }}
  - : With the user's permission through a prompt, turns on a camera or screensharing and/or a microphone on the system and provides a {{domxref("MediaStream")}} containing a video track and/or an audio track with the input.
- {{ domxref("MediaDevices.enumerateDevices()") }}
  - : Obtains an array of information about the media input and output devices available on the system.

## Example

```js
'use strict';

// Put variables in global scope to make them available to the browser console.
var video = document.querySelector('video');
var constraints = window.constraints = {
  audio: false,
  video: true
};
var errorElement = document.querySelector('#errorMsg');

navigator.mediaDevices.getUserMedia(constraints)
.then(function(stream) {
  var videoTracks = stream.getVideoTracks();
  console.log('Got stream with constraints:', constraints);
  console.log('Using video device: ' + videoTracks[0].label);
  stream.onended = function() {
    console.log('Stream ended');
  };
  window.stream = stream; // make variable available to browser console
  video.srcObject = stream;
})
.catch(function(error) {
  if (error.name === 'ConstraintNotSatisfiedError') {
    errorMsg('The resolution ' + constraints.video.width.exact + 'x' +
        constraints.video.width.exact + ' px is not supported by your device.');
  } else if (error.name === 'PermissionDeniedError') {
    errorMsg('Permissions have not been granted to use your camera and ' +
      'microphone, you need to allow the page access to your devices in ' +
      'order for the demo to work.');
  }
  errorMsg('getUserMedia error: ' + error.name, error);
});

function errorMsg(msg, error) {
  errorElement.innerHTML += '<p>' + msg + '</p>';
  if (typeof error !== 'undefined') {
    console.error(error);
  }
}
```

## Especificaciones

{{Specifications}}

## Browser compatibility

{{Compat("api.MediaDevices")}}

## See also

- [WebRTC](/es/docs/WebRTC) - the introductory page to the API
- [Navigator.getUserMedia()](/es/docs/WebRTC/navigator.getUserMedia)
