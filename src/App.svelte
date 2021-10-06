<script>
  const constrains = { audio: true };
  let mediaRecorder = null;
  let chunks = [];

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
          const blob = new Blob(chunks, { type: "audio/ogg; codecs-opus" });
          const audioURL = URL.createObjectURL(blob);

          chunks = [];
          console.log("audioURL: " + audioURL);
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

  <div>Doesn't have any audio stored.</div>

  <div class="storage">
    <span>Audio1</span>
    <audio controls />
    <button>Delete</button>
  </div>

  <div class="storage">
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
    height: auto;
  }
  .storage {
    display: flex;
    align-items: center;
    justify-content: space-evenly;
    margin: 0.5rem;
  }
</style>
