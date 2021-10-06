<script>
  let mediaRecorder = null;
  let chunks = [];

  const constrains = { audio: true };
  navigator.mediaDevices.getUserMedia(constrains).then((stream) => {
    mediaRecorder = new MediaRecorder(stream);

    mediaRecorder.ondataavailable = (e) => {
      chunks.push(e.data);
    };

    mediaRecorder.onstop = (e) => {
      const blob = new Blob(chunks, { type: "audio/ogg; codecs-opus" });
      const audioURL = URL.createObjectURL(blob);

      chunks = [];
      console.log("audioURL: " + audioURL);
    };
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
</style>
