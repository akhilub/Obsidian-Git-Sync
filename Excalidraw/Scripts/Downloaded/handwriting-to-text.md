/*
```javascript
*/
if (!ea.verifyMinimumPluginVersion || !ea.verifyMinimumPluginVersion("1.8.25")) {
  new Notice("This script requires a newer version of Excalidraw. Please install the latest version.");
  return;
}

const socket = new WebSocket('ws://localhost:8765');

console.log('Attempting to connect to WebSocket server...');

socket.onopen = () => {
  console.log('Connected to WebSocket server');
};

socket.onmessage = (event) => {
  console.log('Message received from server:', event.data);
  try {
    const data = JSON.parse(event.data);
    const recognizedText = data.recognized_text;
    insertTextIntoExcalidraw(recognizedText);
  } catch (error) {
    console.error('Error parsing message from server:', error);
  }
};

function captureHandwritingData(handwritingData) {
  console.log('Capturing handwriting data:', handwritingData);
  const message = JSON.stringify(handwritingData);
  console.log('Sending message:', message);
  socket.send(message);
}

function insertTextIntoExcalidraw(text) {
  console.log('Inserting text into Excalidraw:', text);
  const canvas = document.querySelector('.excalidraw-container');
  if (canvas) {
    const textElement = document.createElement('div');
    textElement.innerText = text;
    textElement.style.position = 'absolute';
    textElement.style.left = '10px';
    textElement.style.top = '10px';
    canvas.appendChild(textElement);
  }
}

document.addEventListener('DOMContentLoaded', () => {
  console.log('Document is ready');
  const excalidrawCanvas = document.querySelector('.excalidraw-container');
  if (excalidrawCanvas) {
    console.log('Excalidraw canvas found');
    excalidrawCanvas.addEventListener('pointerup', (event) => {
      console.log('Pointer up event detected');
      const handwritingData = extractHandwritingData(event);
      captureHandwritingData(handwritingData);
    });
  } else {
    console.log('Excalidraw canvas not found');
  }
});

function extractHandwritingData(event) {
  console.log('Extracting handwriting data');
  return {
    x: event.clientX,
    y: event.clientY,
    data: "example"
  };
}

