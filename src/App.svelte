<script>
  import { Button, Icon } from "sveltestrap";

  const constrains = { audio: true };
  let mediaRecorder = null;
  let chunks = [];
  let status = false;
  let recordings = [];

  if (navigator.mediaDevices) {
    console.log("getUserMedia supported.");

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

          console.log(recordings);
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
    <Button color="danger" on:click={record}>Play</Button>
    <Button color="danger" on:click={stop}>Stop</Button>

    {#if status === false}
      <span>Waiting</span>
    {:else}
      <span class="recording">Recording</span>
    {/if}
  </div>

  {#if recordings.length > 0}
    {#each recordings as recording, i}
      <div class="storage">
        <span>{i + 1 + ": " + recording.title}</span>
        <audio controls src={recording.audio} />
        <Button
          class="lixeira"
          style="padding: 0px"
          color=""
          on:click={deleteRecording}><Icon name="trash-fill" /></Button
        >
      </div>
    {/each}
  {:else}
    <div>Doesn't have any audio stored.</div>
  {/if}
</body>

<style>
  body {
    background-color: burlywood;
    max-width: 45rem;
    margin: auto;
    text-align: center;
    height: auto;
  }
  .storage {
    display: flex;
    align-items: center;
    justify-content: space-evenly;
    margin: 0.5rem;
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
