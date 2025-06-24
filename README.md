```javascript

const { app, BrowserWindow } = require('electron');
const path = require('path');

function createWindow() {
  const win = new BrowserWindow({
    width: 1000,
    height: 800,
    webPreferences: {
      contextIsolation: true,
    },
  });

  win.loadURL(`file://${path.join(__dirname, 'build/index.html')}`);
  win.webContents.openDevTools(); // Debug
}

app.whenReady().then(createWindow);

app.on('window-all-closed', () => {
  if (process.platform !== 'darwin') app.quit();
});


```
