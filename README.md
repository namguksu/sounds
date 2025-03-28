<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Dynamic Playsound Table</title>
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css">
  <style>
    .pitchControl {
      width: 80px;
      height: 2px;
    }
  </style>
</head>
<body>
<div>
  <table style="border-collapse: collapse; width: 100%;" border="1">
    <tbody>
      
<tr>
  <td style="width: 70%;">ambient.weather.thunder</td>
  <td style="width: 15%;">
    <span class="icon"><i class="fa fa-volume-up fa-2x" style="color:grey;opacity:.5;cursor:pointer;"></i></span>
    <div class="audio-group">
      <audio><source src="https://github.com/namguksu/sounds/blob/main/ambient.nether.basalt_deltas.active1.mp3" type="audio/mp3"></audio>
    </div>
  </td>
  <td style="width: 15%;"><div style="margin: 10px 0;"><input type="range" class="pitchControl" min="0.1" max="2" step="0.1" value="1"><span class="pitchValue">1.0x</span></div></td>
</tr>
   </tbody>
  </table>
</div>
<script>
  document.addEventListener("DOMContentLoaded", function () {
    const icons = document.querySelectorAll(".icon i");
    const audioGroups = document.querySelectorAll(".audio-group");
    const pitchControls = document.querySelectorAll(".pitchControl");
    const pitchValues = document.querySelectorAll(".pitchValue");

    const currentIndexes = Array.from({ length: audioGroups.length }, () => 0);

    pitchControls.forEach((pitchControl, i) => {
      pitchControl.addEventListener("input", () => {
        const newVal = parseFloat(pitchControl.value);
        pitchValues[i].textContent = newVal.toFixed(1) + "x";
        const audios = audioGroups[i].querySelectorAll("audio");
        audios.forEach(audio => {
          audio.playbackRate = newVal;
          if ('preservesPitch' in audio) audio.preservesPitch = false;
          if ('mozPreservesPitch' in audio) audio.mozPreservesPitch = false;
          if ('webkitPreservesPitch' in audio) audio.webkitPreservesPitch = false;
        });
      });
    });

    icons.forEach((icon, index) => {
      icon.addEventListener("click", function () {
        const audioGroup = audioGroups[index];
        const audios = audioGroup.querySelectorAll("audio");
        const pitchControl = pitchControls[index];
        const pitchValue = pitchValues[index];
        const currentIndex = currentIndexes[index];
        const currentAudio = audios[currentIndex];

        if (!currentAudio.paused && !currentAudio.ended) {
          currentAudio.pause();
          currentAudio.currentTime = 0;
          icon.style.opacity = "0.5";
          return;
        }

        audioGroups.forEach(group => {
          group.querySelectorAll("audio").forEach(audio => {
            audio.pause();
            audio.currentTime = 0;
          });
        });
        icons.forEach(ic => ic.style.opacity = "0.5");

        const nextIndex = (currentIndex + 1) % audios.length;
        const nextAudio = audios[nextIndex];

        const newPitch = parseFloat(pitchControl.value);
        pitchValue.textContent = newPitch.toFixed(1) + "x";
        audios.forEach(audio => {
          audio.playbackRate = newPitch;
          if ('preservesPitch' in audio) audio.preservesPitch = false;
          if ('mozPreservesPitch' in audio) audio.mozPreservesPitch = false;
          if ('webkitPreservesPitch' in audio) audio.webkitPreservesPitch = false;
        });

        nextAudio.play();
        icon.style.opacity = "1";
        currentIndexes[index] = nextIndex;
      });
    });
  });
</script>
</body>
</html>
