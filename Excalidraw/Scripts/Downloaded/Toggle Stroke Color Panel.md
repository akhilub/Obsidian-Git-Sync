---
name: Toggle Stroke Color Panel
id: toggle-stroke-color-panel
description: Toggles the stroke color panel in Excalidraw
author: Akhil Singh Chauhan
version: 1.0
minAppVersion: 2.4.0
---

/*
```javascript
*/
if (ea.verifyMinimumPluginVersion && ea.verifyMinimumPluginVersion("2.4.0")) {
  let isPanelOpen = false; // Track panel state

  async function toggleStrokeColorPanel() {
    await ea.setView("active"); // Ensure Excalidraw is active

    // Get Excalidraw API
    const api = ea.getExcalidrawAPI();
    if (!api) {
      new Notice("Excalidraw API not available. Ensure Excalidraw is open.");
      return;
    }

    if (isPanelOpen) {
      // Close the Edit button and return to previous state
      let editButton = document.querySelector('button[aria-label="Edit"]');
      if (editButton) {
        editButton.click();
      }
      isPanelOpen = false;
      return;
    }

    // Select the Draw (freedraw) tool by updating appState
    api.updateScene({
      appState: { activeTool: { type: "freedraw" } }
    });

    // Wait briefly to allow UI updates
    await new Promise((resolve) => setTimeout(resolve, 100));

    // Open the Edit button
    let editButton = document.querySelector('button[aria-label="Edit"]');
    if (editButton) {
      editButton.click();
    }

    // Wait for UI update
    await new Promise((resolve) => setTimeout(resolve, 100));

    // Find and open the Stroke Color Panel
    let strokeColorButton = document.querySelector(
      'button[aria-label="Stroke"], button[title="Show stroke color picker"]'
    );
    if (strokeColorButton) {
      strokeColorButton.click();
    } else {
      new Notice("Stroke Color Panel button not found. Ensure Excalidraw is open.");
      return;
    }
    
    // // Wait for color picker to appear
    // await new Promise((resolve) => setTimeout(resolve, 100));

    // // Select the first color from the color picker
    // let firstColorButton = document.querySelector(".color-picker__top-picks .color-picker__button");
    // if (firstColorButton) {
    //   firstColorButton.click();
    // } else {
    //   new Notice("Color Picker not found. Ensure Excalidraw is open.");
    // }

    isPanelOpen = true; // Mark panel as open
  }

  toggleStrokeColorPanel(); // Run the function
} else {
  new Notice("This script requires Excalidraw version 2.4.0 or higher.");
}
