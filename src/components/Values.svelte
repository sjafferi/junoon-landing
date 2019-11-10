<script>
  import { fade } from "svelte/transition";
  let values = [
    {
      key: "agenda",
      description: "Super simple planning, week by week."
    },
    {
      key: "accountability",
      description: "Stay on track with a daily review."
    },
    {
      key: "analysis",
      description: "Measure what matters with metrics."
    }
  ];
  let currHover;
  let current = "agenda";
  let lastCarouselUpdatedAt;
  const CAROUSEL_INTERVAL = 4000;

  setInterval(() => {
    if (
      !lastCarouselUpdatedAt ||
      Date.now() - lastCarouselUpdatedAt >= CAROUSEL_INTERVAL
    ) {
      let curr_idx = values.findIndex(({ key }) => current === key);
      current = values[++curr_idx % values.length].key;
      lastCarouselUpdatedAt = Date.now();
    }
  }, (CAROUSEL_INTERVAL + 10) / 2);
</script>

<style>
  .titles {
    display: flex;
    justify-content: center;
  }

  figure {
    margin: 0.5em 0 1em 0;
    padding: 1.25em;
    display: flex;
    flex-direction: column;
    justify-content: flex-end;
    align-items: center;
    background: white;
  }

  figure:hover {
    cursor: pointer;
    user-select: none;
    transition: background 120ms ease-in 0s;
    background: rgba(55, 53, 47, 0.025);
  }

  img {
    width: 300px;
  }

  .titles img {
    height: 200px;
    margin: 0 0 1em 0;
  }

  figcaption {
    display: flex;
    flex-direction: column;
    align-items: center;
    font-size: 1.25em;
    text-align: center;
    text-transform: capitalize;
    padding-bottom: 1px;
  }

  figcaption p:first-child {
    width: fit-content;
    border-bottom: 1.5px solid rgba(0, 0, 0, 0);
    transition: border-bottom-color 225ms ease-in-out 0s;
  }

  figcaption p:last-child {
    font-size: 0.75em;
    text-transform: none;
    color: #787878;
  }

  figure.selected figcaption p:first-child {
    border-bottom-color: rgba(0, 0, 0, 1);
  }

  .preview {
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  .preview img {
    width: 70vw;
    box-shadow: rgba(84, 70, 35, 0.15) 0px 2px 8px,
      rgba(84, 70, 35, 0.15) 0px 1px 3px;
  }

  @media (min-width: 1500px) {
    .preview img {
      width: 50vw;
    }
  }
</style>

<div class="values">
  <div class="titles">
    {#each values as value}
      <figure
        class={(currHover ? currHover === value.key : value.key === current) ? 'selected' : ''}
        on:click={() => {
          current = value.key;
          lastCarouselUpdatedAt = Date.now();
        }}
        on:mouseover={() => (currHover = value.key)}
        on:mouseout={() => (currHover = currHover === value.key ? undefined : currHover)}>
        <img alt={value.key} src="images/values/{value.key}-1.svg" />
        <figcaption>
          <p>{value.key}</p>
          <p>{value.description}</p>
        </figcaption>
      </figure>
    {/each}
  </div>

  <div class="preview">
    <img src="images/values/previews/safari-toolbar.png" alt="Safari toolbar" />
    {#each values as value}
      {#if value.key === current}
        <img
          in:fade={{ delay: 0, duration: 200 }}
          out:fade={{ delay: 0, duration: 100 }}
          alt={value.key}
          src="images/values/previews/{value.key}.png" />
      {/if}
    {/each}
  </div>
</div>