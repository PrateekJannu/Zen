<script>
  import { messages } from "$lib/store";
  import { afterUpdate } from "svelte";

  let messageContainer;

  afterUpdate(() => {
    if ($messages && $messages.length > 0) {
      messageContainer.scrollTo({
        top: messageContainer.scrollHeight,
        behavior: "smooth"
      });
    }
  });

  function typewriter(node) {
    const text = node.textContent;
    node.textContent = '';
    let index = 0;

    function type() {
      if (index < text.length) {
        node.textContent += text.charAt(index);
        index++;
        setTimeout(type, 5);
      }
    }

    type();
  }
</script>

<div
  id="message-container"
  class="flex flex-col flex-1 gap-2 overflow-y-auto rounded-lg"
  bind:this={messageContainer}
>
  {#if $messages !== null}
    <div class="flex flex-col">
      {#each $messages as message}
        <div class="flex items-start gap-2 px-2 py-4">
          {#if message.from_Zen}
            <img
              src="/assets/google-gemini-icon.png"
              alt="Zen's Avatar"
              class="flex-shrink-0 avatar"
              style="width: 32px; height: 32px;"
            />
          {:else}
          <!--
            <img
              src="/assets/user-avatar.svg"
              alt="User's Avatar"
              class="flex-shrink-0 avatar"
              style="width: 28px; height: 28px;"
            />
        -->
          {/if}
          <div class="flex flex-col w-full text-sm">
            <p class="text-base font-semibold text-gray-400">
              {message.from_Zen ? "Zen (Software Engineer)" : "You"}
            </p>
            {#if message.from_Zen && message.message.startsWith("{")}
              <div class="flex flex-col w-full gap-5" contenteditable="false">
                {@html `<strong>Here's my step-by-step plan:</strong>`}
                <div class="flex flex-col gap-3">
                {#if JSON.parse(message.message)}
                  {#each Object.entries(JSON.parse(message.message)) as [step, description]}
                    <div class="flex items-center gap-2">
                      <input type="checkbox" id="step-{step}" disabled />
                      <label for="step-{step}" class="cursor-auto"><strong>Step {step}</strong>: {description}</label>
                    </div>
                  {/each}
                {/if}
                </div>
              </div>
            {:else if /https?:\/\/[^\s]+/.test(message.message)}
              <div class="w-full cursor-auto" contenteditable="false">
                {@html message.message.replace(
                  /(https?:\/\/[^\s]+)/g,
                  '<u><a href="$1" target="_blank" style="font-weight: bold;">$1</a></u>'
                )}
              </div>
            {:else}
              {#if message.from_Zen}
                <div
                  class="w-full"
                  contenteditable="false"
                  use:typewriter
                  bind:innerHTML={message.message}
                ></div>
              {:else}
                <div
                  class="w-full"
                  contenteditable="false"
                  bind:innerHTML={message.message}
                ></div>
              {/if}
            {/if}
          </div>
        </div>
      {/each}
    </div>
  {/if}
</div>

<style>
  #message-container {
    scrollbar-width: none;
  }
  .text-base {
    font-size: 18px /* 16px */;
    line-height: 24px /* 24px */;
}

  .avatar {
    border-radius: 0; /* Makes the avatars square */
  }

  input[type="checkbox"] {
    appearance: none;
    -webkit-appearance: none;
    -moz-appearance: none;
    -ms-appearance: none;
    -o-appearance: none;
    width: 12px;
    height: 12px;
    border: 2px solid black;
    border-radius: 4px;
  }
</style>
