<div align="center">

## IP Multicasting with Winsock control


</div>

### Description

Implements IP multicasting
 
### More Info
 


<span>             |<span>
---                |---
**Submitted On**   |
**By**             |[Iulianos Kakulidis](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByAuthor/iulianos-kakulidis.md)
**Level**          |Intermediate
**User Rating**    |4.5 (27 globes from 6 users)
**Compatibility**  |VB 5\.0, VB 6\.0
**Category**       |[Internet/ HTML](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByCategory/internet-html__1-34.md)
**World**          |[Visual Basic](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByWorld/visual-basic.md)
**Archive File**   |[](https://github.com/Planet-Source-Code/iulianos-kakulidis-ip-multicasting-with-winsock-control__1-29005/archive/master.zip)

### API Declarations

```
Public Type ipm_req
  ipm_multiaddr As Long
  ipm_interface As Long
End Type
Public Declare Function setsockopt Lib "wsock32" _
(ByVal s As Integer, ByVal level As Integer, _
ByVal optname As Integer, ByRef optval As Any, ByVal optlen As Integer) As Integer
Public Declare Function inet_addr Lib "wsock32" _
	(ByVal cp As String) As Long
```


### Source Code

```
It's easy to add IP Multicasting functionality to VB's Winsock control. First, create a new standard EXE project, name it Sender. Set the Caption property of the form to MSender. Draw on the form TextBox and WinSock controls. Set the Protocol property  of WinSock to sckUDPProtocol, RemoteHost to 224.0.0.1, RemotePort to 9000. Add the code bellow to the form and save project.
Private Sub Form_Load()
  Winsock1.Bind 5000
End Sub
Private Sub Text1_KeyDown(KeyCode As Integer, Shift As Integer)
  If KeyCode = vbKeyReturn Then
    Winsock1.SendData Text1.Text
    Text1.SelStart = 0
    Text1.SelLength = Len(Text1.Text)
  End If
End Sub
	Now, create new project, name it Listener, Set the Caption property of the form to MListener. Draw on the form TextBox and WinSock controls. Set the Protocol property of WinSock to sckUDPProtocol. Set the property MultiLine of the TextBox to true, ScrollBars to 3 (both). Add the code bellow to the form.
Private Sub Form_Load()
  Dim ipmreq As ipm_req
  Winsock1.Bind 9000
  ipmreq.ipm_multiaddr = inet_addr("224.0.0.1")
  ipmreq.ipm_interface = 0
  '  join group
  setsockopt Winsock1.SocketHandle, _
    0, 5, ipmreq, Len(ipmreq)
End Sub
Private Sub Winsock1_DataArrival(ByVal bytesTotal As Long)
  Dim stdata As String
  Winsock1.GetData stdata
  Text1.Text = Text1.Text & Chr$(13) & Chr$(10) & stdata
End Sub
	Add the module to the Listener project with the code bellow, save the project.
Public Type ipm_req
  ipm_multiaddr As Long
  ipm_interface As Long
End Type
Public Declare Function setsockopt Lib "wsock32" _
  (ByVal s As Integer, ByVal level As Integer, _
  ByVal optname As Integer, ByRef optval As Any, _
  ByVal optlen As Integer) As Integer
Public Declare Function inet_addr Lib "wsock32" _
	(ByVal cp As String) As Long
	Run Sender and Listener applications. Type message in Sender's TextBox, press Enter, the same text will appear in the TextBox on the Listener's form. Tested on local network
```

