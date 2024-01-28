<script setup lang="ts">
import Button from "@/components/ui/button/Button.vue";
import Input from "@/components/ui/input/Input.vue";
import { useSignalRInvoke } from "@/signalR/connection";
import { useGameStore } from "@/stores/game-store";
import { ref } from "vue";

const { setUserInfo, userName: storedUserName } = useGameStore();

let roomName = ref("");
let userName = ref(storedUserName || "");

const { execute: joinRoomInvoke, onInvokeResult } =
  useSignalRInvoke("JoinRoom");

function handleJoinRoom() {
  const room = roomName.value.trim();
  const user = userName.value.trim();

  if (!room || !user) {
    console.error("Podaj nazwę pokoju i nazwę użytkownika");
    return;
  }

  joinRoomInvoke(room, user);
  onInvokeResult(console.log);
  setUserInfo({
    roomName: room,
    userName: user,
  });
}
</script>

<template>
  <section class="w-[100%] h-[100%] flex justify-center mt-[100px]">
    <h3 class="font-bold text-[32px] text-emerald-400">Zgadnij liczbę!</h3>
    <form
      @submit.prevent="handleJoinRoom"
      className="flex bg-white gap-4  p-4 max-w-lg w-full rounded mx-auto fixed top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2"
    >
      <Input
        v-model="roomName"
        placeholder="Wpisz nazwę pokoju"
        autoFocus
        class="rounded"
      />
      <Input v-model="userName" placeholder="Wpisz nazwę użytkownika" />
      <Button type="submit" class="hover:bg-emerald-400 bg-zinc-400"
        >Graj</Button
      >
    </form>
  </section>
</template>
