<script>
  import { onMount } from "svelte";

  import Neutralino from "@neutralinojs/lib";

  import Button from "../components/Button.svelte";

  let number = 0;
  let computerInfo = {};

  const addNumber = () => {
    number++;
  };

  const getComputerInfo = async () => {
    const arch = await Neutralino.computer.getArch();
    const memoryInfo = await Neutralino.computer.getMemoryInfo();
    const ram = Math.round(memoryInfo.physical.total / 1024 / 1024 / 1024);
    const osInfo = await Neutralino.computer.getOSInfo();
    const os = `${osInfo.name} ${osInfo.version}`;

    computerInfo = { ...computerInfo, arch, ram, os };
  };

  onMount(() => {
    Neutralino.init();

    getComputerInfo();
  });
</script>

<div class="flex flex-col gap-5 w-full h-screen justify-center items-center">
  <span class="text-3xl font-bold text-blue-500">{number}</span>
  <Button on:click={addNumber} text="Add number"></Button>

  <div class="text-xs flex flex-col bg-gray-100 px-2 py-1 my-5">
    {#if computerInfo.arch}
      <pre>Architecture: {computerInfo.arch}</pre>
    {/if}
    {#if computerInfo.ram}
      <pre>RAM: {computerInfo.ram} GB</pre>
    {/if}
    {#if computerInfo.os}
      <pre>OS: {computerInfo.os}</pre>
    {/if}
  </div>
</div>
