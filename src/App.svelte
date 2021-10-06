<script>
  import AudioList from "./AudioList.svelte";
  import { Button } from "sveltestrap";

  const constrains = { audio: true };
  let mediaRecorder = null;
  let chunks = [];
  let status = false;
  let recordings = [];

  if (navigator.mediaDevices) {
    navigator.mediaDevices
      .getUserMedia(constrains)
      .then((stream) => {
        mediaRecorder = new MediaRecorder(stream);

        mediaRecorder.ondataavailable = (e) => {
          chunks.push(e.data);
        };

        mediaRecorder.onstop = (e) => {
          const clipName = prompt("Enter a name for your audio clip");

          if (clipName === null || clipName === undefined || clipName === "") {
            clipName = "";
          }

          const blob = new Blob(chunks, { type: "audio/ogg; codecs-opus" });
          const audioURL = URL.createObjectURL(blob);

          chunks = [];

          recordings = [
            ...recordings,
            {
              title: clipName,
              audio: audioURL,
            },
          ];
        };
      })
      .catch((err) => {
        alert(
          "Permission denied. To use the recorder you will have to allow access to the microphone."
        );
        console.log("The fallowing error occurred: " + err);
      });
  }

  function record() {
    mediaRecorder.start();
    status = true;
  }

  function stop() {
    mediaRecorder.stop();
    status = false;
  }

  function deleteRecording(item) {
    recordings = recordings.filter((i) => i !== item);
  }
</script>

<body>
  <h1>Voice recorder</h1>

  <div class="buttons">
    <Button disabled={status === true} color="danger" on:click={record}
      >Play</Button
    >
    <Button disabled={status === false} color="danger" on:click={stop}
      >Stop</Button
    >

    {#if status === false}
      <span>Waiting</span>
    {:else}
      <span class="recording">Recording</span>
    {/if}
  </div>

  {#if recordings.length > 0}
    {#each recordings as recording, i}
      <AudioList
        title={i + 1 + ": " + recording.title}
        audio={recording.audio}
        on:deleterecording={() => deleteRecording(recording)}
      />
    {/each}
  {:else}
    <div>Doesn't have any audio stored.</div>
  {/if}
</body>

<style>
  body {
    background-color: burlywood;
    max-width: 35rem;
    min-height: 9rem;
    margin: auto;
    text-align: center;
  }
  h1 {
    margin: 0;
  }
  .buttons {
    margin: 0.5rem 0;
  }
  .recording {
    font-weight: bold;
    animation: recorder 2s infinite;
  }
  @keyframes recorder {
    0% {
      color: darkred;
    }
    50% {
      color: red;
    }
    100% {
      color: darkred;
    }
  }
</style>
