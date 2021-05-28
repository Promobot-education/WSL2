## Подготовка к работе

:warning: Требуется **Windows 10**

1. Включить необходимые компоненты (команды выполняются в PowerShell от имени администратора):

```PowerShell
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```

```PowerShell
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

**Перезагрузить ПК**

2. Скачать и установить последнее обновление необходимых компонентов. [Ссылка](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi)

3. Сделать использование WSL2 по умолчанию (команда выполняется в PowerShell от имени администратора):

```PowerShell
wsl --set-default-version 2
```

4. Скачать дистрибутив (200 MB) Ubuntu 16.04. [Ссылка](https://aka.ms/wsl-ubuntu-1604) 

5. Распаковать скачанный архив. Запустить **ubuntu1604.exe**

6. Ввести желаемое имя пользователя и пароль для учетной записи

7. ОС Ubuntu готова к работе

[<img src="https://github.com/Promobot-education/WSL2/blob/main/docs/res/start.png" width="400"/>]()

## Запуск графических приложений

1. Установить на системе Windows X-сервер [Ссылка](https://sourceforge.net/projects/vcxsrv/files/latest/download)

2. На системе Windows запустить ярлык XLaunch (на рабочем столе) и установить параметры как указано на изображениях ниже:

[<img src="https://github.com/Promobot-education/WSL2/blob/main/docs/res/1.png" width="400"/>]()

[<img src="https://github.com/Promobot-education/WSL2/blob/main/docs/res/2.png" width="400"/>]()

[<img src="https://github.com/Promobot-education/WSL2/blob/main/docs/res/3.png" width="400"/>]()

3. Сохранить полученную конфигурацию (для удобства на рабочий стол). Сохраненный файл запускать перед началом работы с ОС Ubuntu

[<img src="https://github.com/Promobot-education/WSL2/blob/main/docs/res/4.png" width="400"/>]()

Запущенный сервер в трее Windows:

[<img src="https://github.com/Promobot-education/WSL2/blob/main/docs/res/tray.png" width="200"/>]()

4. В ОС Ubuntu выполнить следующие команды:

```bash
echo "export LIBGL_ALWAYS_INDIRECT=0" >> ~/.bashrc 

echo "export DISPLAY=$(awk '/nameserver / {print $2; exit}' /etc/resolv.conf 2>/dev/null):0" >> ~/.bashrc 

echo "export HOST_ADDR=$(awk '/nameserver / {print $2; exit}' /etc/resolv.conf 2>/dev/null) " >> ~/.bashrc 
```

## Работа с реальными устройствами (USB) из ОС Ubuntu

По умолчанию WSL2 не позволяет подключаться к каким-либо устройствами по USB

Для возможности работать с релаьными устройствами по USB необходимо:

1. Подключить устройство (интерфейсный блок Robox или Rooky) к ПК

2. Скачать с репозитория [сервер (TCP <-> COM)](https://github.com/Promobot-education/WSL2/blob/main/utils/Server.py)

3. Запустить на ОС Windows сервер (команда выполняется в PowerShell от имени администратора):  

```PowerShell
python .\Server.py
```

4. Запустить на ОС Ubuntu клиент:

```bash
sudo socat -d -d pty,link=/dev/RS_485,raw,echo=0,perm=0666 tcp:$HOST_ADDR:5000
```

5. Создан канал передачи данных USB из Ubuntu в устройство, подключенное к Windows (адрес порта в Ubuntu **/dev/RS_485**)

6. Далее, перед началом работы выполнять пункты **(3)** и **(4)**