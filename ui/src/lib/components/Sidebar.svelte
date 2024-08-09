<script>
  import SidebarButton from "./ui/SidebarButton.svelte";
  import { page } from "$app/stores";

  import {Icons} from "./../icons"
  const GoogleIcon = `
    <svg xmlns="http://www.w3.org/2000/svg" width="40" height="40" viewBox="0 0 512 512">
      <rect width="512" height="512" fill="none" rx="15%" />
      <path fill="#4285f4" d="M386 400c45-42 65-112 53-179H260v74h102c-4 24-18 44-38 57z" />
      <path fill="#34a853" d="M90 341a192 192 0 0 0 296 59l-62-48c-53 35-141 22-171-60z" />
      <path fill="#fbbc02" d="M153 292c-8-25-8-48 0-73l-63-49c-23 46-30 111 0 171z" />
      <path fill="#ea4335" d="M153 219c22-69 116-109 179-50l55-54c-78-75-230-72-297 55z" />
    </svg>`;


  let navItems = [

  {
      icon: GoogleIcon, // Use the imported SVG
      tooltip: "Google",
      route: "/",
    },
    {
      icon: Icons.HOME,
      tooltip: "Home",
      route: "/",
    },
    {
      icon: Icons.SETTINGS,
      tooltip: "Settings",
      route: "/settings",
    },
    {
      icon: Icons.LOGS,
      tooltip: "Logs",
      route: "/logs",
    },
  ];
</script>

<div
  class="flex flex-col text-tertiary mx-2 my-4 gap-6 items-center bg-secondary rounded-xl p-6"
>
  {#each navItems as { icon, tooltip, route }, i}
    <SidebarButton
      icon={icon}
      href={route}
      {tooltip}
      isSelected={$page.url.pathname == route}
    />
  {/each}
</div>
