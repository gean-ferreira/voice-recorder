<script>
  let mediaRecorder = null;

  const constrains = { audio: true };
  navigator.mediaDevices.getUserMedia(constrains).then((stream) => {
    mediaRecorder = new MediaRecorder(stream);
  });

  function record() {
    mediaRecorder.start();
    console.log("play button has been activated");
  }

  function stop() {
    mediaRecorder.stop();
    console.log("stop button has been activated");
  }
</script>

<body>
  <h1>Voice recorder</h1>

  <div class="buttons">
    <button on:click={record}>Play</button>
    <button on:click={stop}>Stop</button>
    <span>Waiting</span>
    <span>Recording</span>
  </div>

  <div class="storage">
    <div>Doesn't have any audio stored.</div>
    <span>Audio1</span>
    <audio controls />
    <button>Delete</button>
  </div>
</body>

<style>
  body {
    background-color: burlywood;
    max-width: 45rem;
    margin: auto;
    text-align: center;
  }
  h1,
  .buttons,
  .storage {
    text-align: center;
  }
</style>
