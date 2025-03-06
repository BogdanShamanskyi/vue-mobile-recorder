<template>
  <div class="mobile-video-recorder">
    <div v-if="error" class="error-message">
      {{ error }}
      <button @click="retryCamera">Try Again</button>
    </div>

    <video ref="videoElement" autoplay playsinline v-show="showCamera" class="camera-preview"></video>

    <video
        v-if="isVideoRecorded && recordedVideoUrl"
        :src="recordedVideoUrl"
        controls
        playsinline
        class="recorded-video"
    ></video>

    <div v-if="showCamera" class="controls">
      <button @click="toggleRecording">
        {{ isRecording ? 'Stop Recording' : 'Start Recording' }}
      </button>
    </div>

    <div v-if="isVideoRecorded" class="actions">
      <button @click="downloadVideo">Download</button>
      <button @click="resetRecorder">Record Again</button>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted, nextTick } from 'vue';

const videoElement = ref(null);
const stream = ref(null);
const mediaRecorder = ref(null);
const recordedChunks = ref([]);
const isRecording = ref(false);
const isVideoRecorded = ref(false);
const recordedVideoUrl = ref(null);
const showCamera = ref(false);
const error = ref(null);

const checkSupportedMimeTypes = () => {
  const types = [
    'video/webm;codecs=h264',
    'video/webm',
    'video/mp4;codecs=h264',
    'video/mp4'
  ];
  return types.find(type => MediaRecorder.isTypeSupported(type));
};

const initCamera = async () => {
  try {
    stream.value = await navigator.mediaDevices.getUserMedia({
      video: { facingMode: 'environment' },
      audio: false
    });
    await nextTick();
    videoElement.value.srcObject = stream.value;
    showCamera.value = true;
  } catch (err) {
    error.value = `Camera error: ${err.message}`;
  }
};

const startRecording = () => {
  recordedChunks.value = [];
  const mimeType = checkSupportedMimeTypes();
  mediaRecorder.value = new MediaRecorder(stream.value, { mimeType });

  mediaRecorder.value.ondataavailable = (e) => {
    if (e.data.size > 0) recordedChunks.value.push(e.data);
  };

  mediaRecorder.value.onstop = () => {
    const blob = new Blob(recordedChunks.value, { type: mimeType });
    recordedVideoUrl.value = URL.createObjectURL(blob);
    isVideoRecorded.value = true;
    showCamera.value = false;
  };

  mediaRecorder.value.start();
  isRecording.value = true;
};

const stopRecording = () => {
  mediaRecorder.value.stop();
  isRecording.value = false;
};

const toggleRecording = () => {
  if (isRecording.value) {
    stopRecording();
  } else {
    startRecording();
  }
};

const resetRecorder = () => {
  recordedVideoUrl.value = null;
  isVideoRecorded.value = false;
  showCamera.value = true;
};

const retryCamera = () => {
  error.value = null;
  initCamera();
};

const downloadVideo = () => {
  const link = document.createElement('a');
  link.href = recordedVideoUrl.value;
  link.download = 'recorded-video.webm';
  link.click();
};

onMounted(() => {
  if (!navigator.mediaDevices?.getUserMedia) {
    error.value = 'Camera not supported on this device.';
  } else {
    initCamera();
  }
});

onUnmounted(() => {
  stream.value?.getTracks().forEach(track => track.stop());
});
</script>

<style scoped>
.mobile-video-recorder {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 20px;
  padding: 20px;
}

.camera-preview, .recorded-video {
  width: 100%;
  max-width: 480px;
  border-radius: 12px;
  background: #000;
}

.controls button, .actions button {
  padding: 10px 20px;
  margin: 5px;
  border: none;
  border-radius: 8px;
  background-color: #1B2A47;
  color: white;
  cursor: pointer;
}

.error-message {
  color: red;
}
</style>
