<script>
  import ReactionBar from "./ReactionBar.svelte";
  export let isAdmin = true; // server verifies
  let isReactionDrawerOpen = false;
  export let isWinner = false;
  export let imgURL = "";
  export let reaction = {};
  export let imageID = "";
  export let votes = { upvotes: 0, downvotes: 0 };
  export let upvote = () => {};
  export let downvote = () => {};
  export let voted = "";
  export let updateSelectedReaction = () => {};
  export let clearSelectedReaction = () => {};

  let reactionElements = [
    "https://cdn.lordicon.com/lupuorrc.json",

    "https://cdn.lordicon.com/dzllstvg.json",

    "https://cdn.lordicon.com/rjzlnunf.json",

    "https://cdn.lordicon.com/hrqwmuhr.json",
  ];
  let reactionback = [
    "bg-indigo-100",
    "bg-yellow-100",
    "bg-red-100",
    "bg-gray-100",
  ];
</script>

{#if isAdmin}
  <div class="relative">
    <div class="max-w-lg">
      <div
        on:click={() => {
          window.location.href = imgURL;
        }}
        class="absolute right-4 top-4 p-1 bg-gray-800  hover:bg-black text-white hover:text-primary transition-all   rounded-xl "
      >
        <svg
          xmlns="http://www.w3.org/2000/svg"
          class="h-6 w-6 ml-auto  transition-all  modal-button"
          fill="none"
          viewBox="0 0 24 24"
          stroke="currentColor"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke-width="2"
            d="M4 8V4m0 0h4M4 4l5 5m11-1V4m0 0h-4m4 0l-5 5M4 16v4m0 0h4m-4 0l5-5m11 5l-5-5m5 5v-4m0 4h-4"
          />
        </svg>
      </div>
      {#if isWinner}
        <div data-tip="Winner Image!" class="tooltip left-16">
          <div
            class="absolute top-4 bg-gray-800 backdrop-opacity-30 -left-12 p-2  rounded-full"
          >
            <svg
              xmlns="http://www.w3.org/2000/svg"
              class="h-6 w-6"
              fill="none"
              viewBox="0 0 24 24"
              stroke="yellow"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="1.2"
                d="M5 3v4M3 5h4M6 17v4m-2-2h4m5-16l2.286 6.857L21 12l-5.714 2.143L13 21l-2.286-6.857L5 12l5.714-2.143L13 3z"
              />
            </svg>
          </div>
        </div>
      {/if}
      <div class="overflow-x-auto flex rounded-xl snap">
        <div class="flex-none w-full min-w-0 snapChild">
          <img
            class={`h-96 w-full  shadow-lg object-cover  `}
            src={imgURL}
            alt="dff"
          />
        </div>
        {#each Object.keys(reaction) as r, index}
          <div
            class={`flex-none flex w-full ${reactionback[index]}  snapChild`}
          >
            <div class="flex items-center justify-center flex-col  w-full ">
              <lord-icon
                src={reactionElements[index]}
                trigger="hover"
                colors="primary:#121331,secondary:#121331"
                class=" w-14 h-14 lg:w-20 lg:h-20"
              />
              <div>{reaction[r]} reactions</div>
            </div>
          </div>
        {/each}
      </div>

      <div class="flex gap-3 mt-2 mb-4">
        <div
          class={`flex items-center hover:text-green-500 ${
            voted === "upvoted" ? "text-green-500" : ""
          }  transition-all `}
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            class="h-6 w-6"
            fill="none"
            viewBox="0 0 24 24"
            stroke="currentColor"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M7 11l5-5m0 0l5 5m-5-5v12"
            />
          </svg>
          <div>{votes.upvotes}</div>
        </div>

        <div
          class={`flex items-center hover:text-red-500 ${
            voted === "downvoted" ? "text-red-500" : ""
          }  transition-all `}
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            class="h-6 w-6 rotate-180 "
            fill="none"
            viewBox="0 0 24 24"
            stroke="currentColor"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M7 11l5-5m0 0l5 5m-5-5v12"
            />
          </svg>
          <div>{votes.downvotes}</div>
        </div>
        <div
          class={votes.upvotes - votes.downvotes >= 0
            ? "ml-auto text-green-400 font-bold flex items-center gap-3"
            : "ml-auto text-red-400 font-bold flex items-center gap-3"}
        >
          {#if votes.upvotes - votes.downvotes < 0}
            <svg
              xmlns="http://www.w3.org/2000/svg"
              class="h-6 w-6"
              fill="none"
              viewBox="0 0 24 24"
              stroke="currentColor"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M13 17h8m0 0V9m0 8l-8-8-4 4-6-6"
              />
            </svg>
          {:else}
            <svg
              xmlns="http://www.w3.org/2000/svg"
              class="h-6 w-6"
              fill="none"
              viewBox="0 0 24 24"
              stroke="currentColor"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M13 7h8m0 0v8m0-8l-8 8-4-4-6 6"
              />
            </svg>
          {/if}
          {votes.upvotes - votes.downvotes}
        </div>
      </div>
    </div>
  </div>
{:else}
  <div class="">
    <div class="relative">
      <div class="max-w-lg">
        <div
          on:click={() => {
            window.location.href = imgURL;
          }}
          class="absolute right-4 top-4 p-1  hover:bg-black opacity-40  hover:text-primary transition-all   rounded-xl "
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            class="h-6 w-6 ml-auto  transition-all  modal-button"
            fill="none"
            viewBox="0 0 24 24"
            stroke="currentColor"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M4 8V4m0 0h4M4 4l5 5m11-1V4m0 0h-4m4 0l-5 5M4 16v4m0 0h4m-4 0l5-5m11 5l-5-5m5 5v-4m0 4h-4"
            />
          </svg>
        </div>
        {#if isWinner}
          <div data-tip="Winner Image!" class="tooltip left-16">
            <div
              class="absolute top-4 bg-gray-800 backdrop-opacity-30 -left-12 p-2  rounded-full"
            >
              <svg
                xmlns="http://www.w3.org/2000/svg"
                class="h-6 w-6"
                fill="none"
                viewBox="0 0 24 24"
                stroke="yellow"
              >
                <path
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  stroke-width="1.2"
                  d="M5 3v4M3 5h4M6 17v4m-2-2h4m5-16l2.286 6.857L21 12l-5.714 2.143L13 21l-2.286-6.857L5 12l5.714-2.143L13 3z"
                />
              </svg>
            </div>
          </div>
        {/if}
        <img
          class={`rounded-xl h-96 shadow-lg w-full object-cover`}
          src={imgURL}
          alt="dff"
        />
      </div>
    </div>

    <div class="flex gap-3 mt-2 mb-4">
      <div
        on:click={() => upvote(imageID)}
        class={`flex items-center hover:text-green-500 ${
          voted === "upvoted" ? "text-green-500" : ""
        }  transition-all cursor-pointer`}
      >
        <svg
          xmlns="http://www.w3.org/2000/svg"
          class="h-6 w-6"
          fill="none"
          viewBox="0 0 24 24"
          stroke="currentColor"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke-width="2"
            d="M7 11l5-5m0 0l5 5m-5-5v12"
          />
        </svg>
        <div>{votes.upvotes}</div>
      </div>

      <div
        on:click={() => downvote(imageID)}
        class={`flex items-center hover:text-red-500 ${
          voted === "downvoted" ? "text-red-500" : ""
        }  transition-all cursor-pointer`}
      >
        <svg
          xmlns="http://www.w3.org/2000/svg"
          class="h-6 w-6 rotate-180 "
          fill="none"
          viewBox="0 0 24 24"
          stroke="currentColor"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke-width="2"
            d="M7 11l5-5m0 0l5 5m-5-5v12"
          />
        </svg>
        <div>{votes.downvotes}</div>
      </div>
      <div
        on:click={() => (isReactionDrawerOpen = !isReactionDrawerOpen)}
        class={`ml-auto hover:text-primary ${
          isReactionDrawerOpen ? "text-primary" : ""
        } transition-all`}
      >
        <svg
          xmlns="http://www.w3.org/2000/svg"
          class="h-6 w-6 "
          fill="none"
          viewBox="0 0 24 24"
          stroke="currentColor"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke-width="2"
            d="M14.828 14.828a4 4 0 01-5.656 0M9 10h.01M15 10h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z"
          />
        </svg>
      </div>
    </div>
    {#if isReactionDrawerOpen}
      <ReactionBar {updateSelectedReaction} {imageID} {clearSelectedReaction} />
    {/if}
  </div>
{/if}

<style>
  .newfont {
    font-family: "Harmattan", sans-serif;
  }
  .w-400 {
    width: 400px;
  }
  .snap {
    scroll-snap-type: x mandatory;
  }
  .snapChild {
    scroll-snap-align: start;
  }
</style>
