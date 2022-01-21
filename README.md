# crypto_pro_rdp
КриптоПро CSP c доступом к локальным смарт-картам по RDP
Опубликовано Наталья Мовчан on 2020-03-30 13:43
Текущие сертифицированные версии КриптоПро CSP не поддерживают работу с локальными смарт-картами(смарт-картами, вставленными в машину, к которой подключаемся) в RDP-сессии, настроек, позволяющих решить эту задачу Microsoft не предоставляет.

Поддержка функционала доступа к локальным смарт-картам по RDP доступна, начиная с КриптоПро CSP 4.0 R5 и КриптоПро CSP 5.0 R2.

Данная версия реализовывает работу с локальными смарт-картами при подключении по RDP для новых криптопровайдеров:

Crypto-Pro GOST R 34.10-2001 System CSP

Crypto-Pro GOST R 34.10-2012 System CSP

Crypto-Pro GOST R 34.10-2012 Strong System CSP

Учётная запись, которая обращается к локальным смарткартам, должна входить в локальную группу администраторов или «Привилегированные пользователи КриптоПро CSP» (CryptoPro CSP Power Users), которую можно создать при необходимости.

Для работы с данными провайдерами необходимо:

1) Использовать CSP версии КС1 с хранением ключей в памяти приложений, но доустановить службу хранения ключей с помощью панели управления CSP или команды:

msiexec /i {407E5BA7-6406-40BF-A4DC-3654B8F584C1} ADDLOCAL=cpcspr1 для CSP 4.0

msiexec /i {50F91F80-D397-437C-B0C8-62128DE3B55E} ADDLOCAL=cpcspr для CSP 5.0

2) Применить reg-файл, создающий специальный криптопровайдер и модифицирующий параметры службы хранения ключей. В частности, для CSP 4.0 (для CSP 5.0 "ProductCode"="{50F91F80-D397-437C-B0C8-62128DE3B55E}") 

Crypto-Pro GOST R 34.10-2012 System CSP:

 

Windows Registry Editor Version 5.00

 

;Register new CSP with SCARD media and system service endpoint

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Cryptography\Defaults\Provider\Crypto-Pro GOST R 34.10-2012 System CSP]

"CP Module Entry Point"="DllStartServer"

"CP Module Name"="cpcspr.dll"

"Image Path"="C:\\Program Files\\Crypto Pro\\CSP\\cpcsp.dll"

"ImpType"=dword:00000008

"ProductCode"="{407E5BA7-6406-40BF-A4DC-3654B8F584C1}"

"CP Service Name"="CryptoPro CSP Service 0"

"CP Service UUID"="6E57FEEE-08E0-46ad-98C6-266B632C03FE"

"SigInFile"=dword:00000001

"Media"="SCARD"

"Type"=dword:00000050

 

[HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Cryptography\Defaults\Provider\Crypto-Pro GOST R 34.10-2012 System CSP]

"CP Module Entry Point"="DllStartServer"

"CP Module Name"="cpcspr.dll"

"Image Path"="C:\\Program Files (x86)\\Crypto Pro\\CSP\\cpcsp.dll"

"ImpType"=dword:00000008

"ProductCode"="{407E5BA7-6406-40BF-A4DC-3654B8F584C1}"

"CP Service Name"="CryptoPro CSP Service 0"

"CP Service UUID"="6E57FEEE-08E0-46ad-98C6-266B632C03FE"

"SigInFile"=dword:00000001

"Media"="SCARD"

"Type"=dword:00000050

 

;Set service no impersonate

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\cpcsp\Parameters]

"NoImpersonate"=dword:00000001

 

;Disable pin cache

[HKEY_LOCAL_MACHINE\SOFTWARE\Crypto Pro\Cryptography\CurrentVersion\Parameters]

"AuthPositions"=dword:00000020

 

[HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Crypto Pro\Cryptography\CurrentVersion\Parameters]

"AuthPositions"=dword:00000020

 

3) Перезапустить службу:

net stop cpcsp && net start cpcsp

4) Переустановить сертификаты, необходимые для работы по RDP, с привязкой к закрытому ключу на локальной смарт-карте с указанием провайдера Crypto-Pro GOST R 34.10-2012 System CSP.

5) Выключить  службу распространения сертификатов CertPropSvc. (В противном случае сертификаты придется переустанавливать снова)

Важно!

Для работы с  контейнерами через панель КриптоПро CSP нужно в выпадающем списке указывать нужный провайдер.

Не все программные продукты умеют работать с новыми провайдерами.

Поддержка новых провайдеров в панели Инструменты КриптоПро есть, начиная с Криптопро CSP 5.0 R2.

Поддержка новых провайдеров в КриптоАРМ есть, начиная с КриптоАРМ 5.4.3.16.

Данная статья будет дополняться. 
