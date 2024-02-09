import wx
import tarfile
import os

class MainFrame(wx.Frame):
    def __init__(self):
        super().__init__(None, title='Compress File')
        panel = wx.Panel(self)
        button = wx.Button(panel, label='Compress File')
        button.Bind(wx.EVT_BUTTON, self.OnCompress)
        self.Show()

    def OnCompress(self, _):
        openFileDialog = wx.FileDialog(self, "Open", "", "", 
                                       "All files (*.*)|*.*", 
                                       wx.FD_OPEN | wx.FD_FILE_MUST_EXIST)

        if openFileDialog.ShowModal() == wx.ID_CANCEL:
            return

        path = openFileDialog.GetPath()

        saveFileDialog = wx.FileDialog(self, "Save", "", "", 
                                       "TAR.GZ files (*.tar.gz)|*.tar.gz", 
                                       wx.FD_SAVE | wx.FD_OVERWRITE_PROMPT)

        if saveFileDialog.ShowModal() == wx.ID_CANCEL:
            return

        save_path = saveFileDialog.GetPath()

        with tarfile.open(save_path, "w:gz") as tar:
            tar.add(path, arcname=os.path.basename(path))

        wx.MessageBox('File compressed successfully', 'Info', wx.OK | wx.ICON_INFORMATION)

if __name__ == "__main__":
    app = wx.App()
    frame = MainFrame()
    app.MainLoop()
