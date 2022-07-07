<script lang="ts">
  import { readText, writeText } from "@tauri-apps/api/clipboard";

  let msg = "";
  let img = false;

  const readClipboard = async () => {
    try {
      if ((await readText()).length === 0) return; // do nothing

      img = true;
      await writeText("");
      msg = "Cleared!";

      await new Promise((_) => setTimeout(_, 1500)); // play the gif for a bit
    } catch (err) {
      msg = err;
    } finally {
      msg = "";
      img = false;
    }
  };
</script>

<main>
  <p>{msg}</p>
  <!-- svelte-ignore a11y-autofocus -->
  <textarea
    readonly
    autofocus
    style="background-color: rgba(28,28,28,{img ? 0 : 1});"
    on:focus={readClipboard}
  />
  <img src={img ? "/delete.gif" : ""} alt="gif" />
</main>

<style>
  :root {
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Oxygen,
      Ubuntu, Cantarell, "Open Sans", "Helvetica Neue", sans-serif;
  }

  main {
    background-color: #2b2b2b;
    color: white;
    width: 100%;
    height: 100%;
  }

  textarea {
    position: fixed;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    resize: none;
    border-width: 0;
    z-index: 1;
    caret-color: transparent;
    overflow: hidden;
    font-weight: 900;
    cursor: pointer;
  }

  textarea:focus {
    cursor: none;
    outline: none;
  }

  img {
    position: fixed;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    width: 100%;
    height: 100%;
    object-fit: cover;
  }

  p {
    position: fixed;
    bottom: 0;
    left: 0;
    right: 0;
    margin: 0;
    font-weight: 600;
    z-index: 10;
    text-align: center;
    background: rgba(0, 0, 0, 0.5);
  }
</style>
