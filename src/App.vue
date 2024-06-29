<template>
  <div>
    <button @click="openModal" v-if="!connected">Connect Wallet</button>
    <div v-else>
      <div>{{ address }}</div><br>
      <button @click="disconnect">Disconnect</button>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import WebApp from '@twa-dev/sdk';
import { TonConnectUI, toUserFriendlyAddress } from '@tonconnect/ui';

WebApp.ready();

const tonConnectUI = new TonConnectUI({
  manifestUrl: 'https://0xa1d1.github.io/tongate/tonconnect-manifest.json'
});

const connected = ref(tonConnectUI.connected);
const address = ref("");

const updateAddress = (info) => {
  if (info?.account?.address) {
    const addr = toUserFriendlyAddress(info.account.address);
    address.value = addr.substring(0, 4) + "..." + addr.substring(addr.length - 4, addr.length);
  } else {
    address.value = "";
  }
}

const unsubscribe = tonConnectUI.onStatusChange(
  walletAndwalletInfo => {
    connected.value = tonConnectUI.connected;
    if (tonConnectUI.connected) {
      updateAddress(walletAndwalletInfo);
    }
  }
);


const disconnect = async () => {
  await tonConnectUI.disconnect();
}

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
