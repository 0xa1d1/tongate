<template>
  <div>
    <button @click="openModal" id="connect">Launch a TON Jetton</button>
  </div>
</template>

<script setup>
import WebApp from '@twa-dev/sdk';
import {TonConnectUI} from '@tonconnect/ui';

const tonConnectUI = new TonConnectUI({
  buttonRootId: 'connect',
  manifestUrl: 'https://0xa1d1.github.io/tongate/tonconnect-manifest.json'
});

WebApp.ready();

const openModal = async () => {
  try {
    await tonConnectUI.openModal();
  } catch (error) {
    console.error('Failed to open modal:', error);
  }
};
</script>

<style scoped>
button {
  padding: 10px 20px;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

button:hover {
  background-color: #0056b3;
}
</style>
