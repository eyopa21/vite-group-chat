<template>
  <div id="app" ref="root"></div>
</template>

<script setup>
import HelloWorld from './components/HelloWorld.vue';
import { ZegoUIKitPrebuilt } from '@zegocloud/zego-uikit-prebuilt';
import { ref, onMounted } from 'vue'

const root = ref(null)

function randomID(len) {
  let result = '';
  if (result) return result;
  var chars = '12345qwertyuiopasdfgh67890jklmnbvcxzMNBVCZXASDQWERTYHGFUIOLKJP',
    maxPos = chars.length,
    i;
  len = len || 5;
  for (i = 0; i < len; i++) {
    result += chars.charAt(Math.floor(Math.random() * maxPos));
  }
  return result;
}

function getUrlParams(
  url = window.location.href
) {
  let urlStr = url.split('?')[1];
  return new URLSearchParams(urlStr);
}



const roomID = getUrlParams().get('roomID') || randomID(5);
// generate Kit Token
const appID = 1154956889;
const serverSecret = "de3c335d5b49653bfab8bb36fc56a72a";
const kitToken = ZegoUIKitPrebuilt.generateKitTokenForTest(appID, serverSecret, roomID, randomID(5), randomID(5));

// Create instance object from Kit Token.
const zp = ZegoUIKitPrebuilt.create(kitToken);
// start the call
zp.joinRoom({
  container: root.value,
  sharedLinks: [
    {
      name: 'Personal link',
      url:
        window.location.protocol + '//' +
        window.location.host + window.location.pathname +
        '?roomID=' +
        roomID,
    },
  ],
  scenario: {
    mode: [ZegoUIKitPrebuilt.OneONoneCall], // To implement 1-on-1 calls, modify the parameter here to [ZegoUIKitPrebuilt.OneONoneCall].
    //config: ScenarioConfig[ScenarioModel] // Specific configurations in the corresponding scenario.
  },

  //maxUsers: 2,
  showPreJoinView: true,
  preJoinViewConfig: {
    title: 'Are you sure to join the metting'// The title of the prejoin view. Uses "enter Room" by default.
  }
});



</script>