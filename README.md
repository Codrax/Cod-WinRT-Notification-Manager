# Codrut's Windows Runtime for Delphi -> Notification Manager
Notification Manager for advanced notifications in Windows 10/11

## Source and Dependencies
This library is part of the [Codrut's Windows Runtime for Delphi](https://github.com/Codrax/Cod-Windows-Runtime), all units can be found there.

Attention! The app must be registered with a TAppRegistration, for futher detalies, look at the Cod Windows Runtime docs.

## Examples
### Creating the Notification Manager
```
  Manager := TNotificationManager.Create;
```

### Creating a notification
```
  // Create notification
  TToastContentBuilder.Create
    .AddText( TToastValueBindable.Create('title') )
    .AddText( TToastValueString.Create('This is the new notifications engine :)') )
    .AddAudio(TSoundEventValue.NotificationIM, WinFalse)
    .AddHeroImage(TToastValueString.Create('C:\Windows\System32\@facial-recognition-windows-hello.gif'))
    .AddProgressBar(TToastValueString.Create('Downloading...'), TToastValueBindable.Create('download-pos'))
    .AddInputTextBox('editbox-id', 'Enter value', 'Response')
    .AddButton('Cancel', TActivationType.Foreground, 'cancel')
    .AddButton('View more', TActivationType.Foreground, 'view')
    .SetActivationType(TActivationType.Protocol)
    .SetLaunchURI('ms-settings:account')
    .BuildNotificationAndFree(Notif);
  
  // Set tag
  Notif.Tag := 'notification1';

  // Data binded values
  Notif.Data := TNotificationData.Create;
  Notif.Data['title'] := 'Hello world!';
  Notif.Data['download-pos'] := '0';

  // Events (must be defined in your form class)
  Notif.OnActivated := NotifActivated;
  Notif.OnDismissed := NotifDismissed;
```

### Pushing notification
```
  Manager.ShowNotification(Notif);
```


### Hiding notification
```
  Manager.HideNotification(Notif);
```

### Updating notification contents
```
  const DownloadValue = Notif.Data['download-pos'].ToSingle+0.1;
  Notif.Data['download-pos'] := DownloadValue.ToString;
  if DownloadValue >= 1 then
    Notif.Data['title'] := 'Download finalised!';

  // Update
  Manager.UpdateNotification(Notif);
```

### Reading event data
```
procedure TForm1.NotifActivated(Sender: TNotification; Arguments: string; UserInput: TUserInputMap);
var
  Value: string;
begin
  // Get button id
  if Arguments = 'view' then
    // Get value of edit box (if there is one with this id)
    Value := UserInput.GetStringValue('editbox-id');
end;
```

## Pictures
![image](https://github.com/user-attachments/assets/d612e2e9-f706-469a-ac4e-85ee5b434c04)
![anim](https://github.com/Codrax/Cod-Notification-Manager/assets/68193064/33026b0f-b11a-4c27-993e-69f6850db506)

## Important notes
- Do not free the `TNotificationManager` until the app will no longer send notification.
- Do not free the notification until It is no longer needed, because you will no longer be able to hide It. The notification can be reset using the `Reset()`
 method
