
**Update time**：{docsify-updated} 

## Internationalization 
The retCode and retInfo returned by the interface response are not internationalized and are processed by the interface caller.  
The internationalization of the interface involving business data is defined by passing the language parameter in the header. The specific international language code is shown below.  

编码|语言
:-:|:-:
de|德语
fr|法语
it|意大利语
es|西班牙语
sv|瑞典语
fi|芬兰语
da|丹麦语
no|挪威语
nl|荷兰语
ru|俄罗斯语
ed|英语
pl|波兰
cs|捷克
zh-cn|中文简体
zh-tw|中国台湾-繁体
he|希伯来语
el|希腊语
pt|葡萄牙语

## Safety Regulations

1. **Data Security**

The App application should ensure the safe use of the appid, appkey and other information obtained through the Haiji network when accessing the UWS. The risk of being disabled by the platform due to the leakage of the UWS service due to the leakage is the responsibility of the APP developer.  
The data acquired by the App through UWS, etc., must ensure storage security.

2. **Interface access security**

It must be executed in strict accordance with the usage documentation of the UWS interface service.


## Common HTTP Status Code Commentary

Status Code|Meaning|Solutions
:-:|:-|:-
200|(success)</br>The request has been successful, and the response header or data body that the request expects will be returned with this response.|
400|（Wrong request）</br>1. The semantics are incorrect and the current request cannot be understood by the server. The client should not submit this request repeatedly unless it is modified. </br>2, the request parameters are incorrect.|Check the request parameters, methods, etc. correctly against the interface documentation.
401|(authentication error)</br> The request requires authentication. The server may return this response for web pages that require login.|Check the request parameters, methods, etc. correctly against the interface documentation.
403|(prohibited)</br> The server has understood the request but refused to execute it. Unlike the 401 response, authentication does not provide any help, and this request should not be submitted repeatedly.|Check the request parameters, methods, etc. correctly against the interface documentation.
404|(Not found)</br> The request failed, and the requested resource was not found on the server.|Check the request parameters, methods, etc. correctly against the interface documentation.
500|(Intra-server error)</br> The server encountered an unexpected condition that prevented it from completing the processing of the request. In general, this problem will occur when the server's code is wrong.|Troubleshoot the corresponding server error or exception
501|(Not yet implemented)</br> The server does not have the ability to complete the request. For example, the server might return this code when the server does not recognize the request method. |Troubleshoot the corresponding server error or exception
502|(Error Gateway)</br> The server received an invalid response from the upstream server as a gateway or proxy. |Troubleshoot the corresponding server error or exception
503|(Service not available)</br> The server is currently unable to process the request due to temporary server maintenance or overload.|Troubleshoot the corresponding server error or exception
504|(Gateway timeout)</br> When a server working as a gateway or proxy attempts to execute a request, it fails to receive a response from the upstream server (the server identified by the URI, such as HTTP, FTP, LDAP) or the secondary server (for example, DNS).|Troubleshoot the corresponding server error or exception


## Public Error Code
|retCode|	retInfo|Try a solution|
|---|---|---|  
|10000|The login was successful, but the password security level was improved. Please change the password|User uses weak password, please change password|
|00001|The login was successful but the latest version of the privacy agreement was not accepted|Please accept the latest privacy agreement for the old account|
|A00001|	service is not available           |Contact the cloud platform to check if the service is normal|
|A00002|	Network exception                  |Contact the cloud platform to check if the service is normal|
|A00003|	Access or operation timeout        |Contact the cloud platform to check if the service is normal|
|A00004|	Internal System Error              |Contact the cloud platform to check if the service is normal|
|A00005|	Database access exception          |Contact the cloud platform to check if the service is normal|
|A00006|	Unknown exception                  |Contact the cloud platform to check if the service is normal|
|A00007|	Mail service exception             |Contact the cloud platform to check if the service is normal|
|A00008|	Mail failed to send                |Contact the cloud platform to check if the service is normal|
|A00009|	The number of emails sent exceeded |Check whether the required parameter is a valid value (non-null and cannot be an empty string)|
|B00001|	Missing required parameters        |N/A|
|B00002|	Incorrect parameter type           |N/A|
|B00003|	Parameter value is out of range or not enumerated   |N/A|
|B00004|	The parameter does not meet the rule requirements   |N/A|
|B00006|	Incorrect parameter length                          |N/A|
|B00007|	The parameter does not match the interface definition   |N/A|
|C00001|	appId and appKey validation failed                      |N/A|
|C00002|	appServer no access authorization                       |N/A|
|C00003|	Insufficient access                                |N/A|
|C00004|	Insufficient authority                             |N/A|
|C00005|	Repeat request                                     |N/A|
|C00006|	Unknown device type                                 |N/A|
|C00007|	appId configuration information is empty           |N/A|
|C00008|	appKey is empty                                    |N/A|
|D00001|	Sign signature error                               |N/A|
|D00002|	Incorrect username or password                        |N/A|
|D00003|	Token does not exist, not verified by token               |N/A|
|D00004|	Token has expired and has not been verified by token        |N/A|
|D00005|	Token is not created by this application, not verified by token  |N/A|
|D00006|	Session invalidation                                    |N/A|
|D00007|	Not an internal user                              |N/A|
|D00008|	User is not legal                                    |N/A|
|D00009|	Login Failure Number More than Limit, Need Verification Code|N/A|
|D00010|	Account Locked| N/A|
|D00011|	Account Not Activated|  N/A|
|D00012|	Account Already Excit|N/A|
|D00015|	Wrong Verification Code|N/A|
|D00016|Already Logout or Havn't Login|N/A|
|D00017|Account Not Excit|N/A|
|G20003|Unable to get adapter/Application information|N/A|
|G20201|Failed to get the user bound device list|N/A|
|G20202|User Doesn't match the Device| Check if the device id matches the user| 
|G20904|The number of bindings has been exceeded| A user can only bind to 100 devices |
|G20908|Safety check failed| Please re-distribute the network |
|G20910|The non-safety device failed to check|Non-secure device binding:</br>1. Equipment must be online</br>2. The time for reporting version information of the equipment shall be no more than 10 minutes, that is, less than or equal to 10 minutes|
|G20301|Device information query failed|N/A|
|G23401|Device unbound|N/A|
|G23501|User - bound device security verification failed|N/A|
|G24001|The equipment brand model information is empty|N/A|  
|20903|Master data access exception|N/A|






## International Time Zone Description

|Time zone / TimeZone ID|
|---|
|Etc/GMT+12
|Etc/GMT+11
|Pacific/Midway
|Pacific/Niue
|Pacific/Pago_Pago
|Pacific/Samoa
|US/Samoa
|America/Adak
|America/Atka
|Etc/GMT+10
|HST
|Pacific/Honolulu
|Pacific/Johnston
|Pacific/Rarotonga
|Pacific/Tahiti
|SystemV/HST10
|US/Aleutian
|US/Hawaii
|Pacific/Marquesas
|AST
|America/Anchorage
|America/Juneau
|America/Nome
|America/Sitka
|America/Yakutat
|Etc/GMT+9
|Pacific/Gambier
|SystemV/YST9
|SystemV/YST9YDT
|US/Alaska
|America/Dawson
|America/Ensenada
|America/Los_Angeles
|America/Metlakatla
|America/Santa_Isabel
|America/Tijuana
|America/Vancouver
|America/Whitehorse
|Canada/Pacific
|Canada/Yukon
|Etc/GMT+8
|Mexico/BajaNorte
|PST
|PST8PDT
|Pacific/Pitcairn
|SystemV/PST8
|SystemV/PST8PDT
|US/Pacific
|US/Pacific-New
|America/Boise
|America/Cambridge_Bay
|America/Chihuahua
|America/Creston
|America/Dawson_Creek
|America/Denver
|America/Edmonton
|America/Hermosillo
|America/Inuvik
|America/Mazatlan
|America/Ojinaga
|America/Phoenix
|America/Shiprock
|America/Yellowknife
|Canada/Mountain
|Etc/GMT+7
|MST
|MST7MDT
|Mexico/BajaSur
|Navajo
|PNT
|SystemV/MST7
|SystemV/MST7MDT
|US/Arizona
|US/Mountain
|America/Bahia_Banderas
|America/Belize
|America/Chicago
|America/Costa_Rica
|America/El_Salvador
|America/Guatemala
|America/Indiana/Knox
|America/Indiana/Tell_City
|America/Knox_IN
|America/Managua
|America/Matamoros
|America/Menominee
|America/Merida
|America/Mexico_City
|America/Monterrey
|America/North_Dakota/Beulah
|America/North_Dakota/Center
|America/North_Dakota/New_Salem
|America/Rainy_River
|America/Rankin_Inlet
|America/Regina
|America/Resolute
|America/Swift_Current
|America/Tegucigalpa
|America/Winnipeg
|CST
|CST6CDT
|Canada/Central
|Canada/East-Saskatchewan
|Canada/Saskatchewan
|Etc/GMT+6
|Mexico/General
|Pacific/Galapagos
|SystemV/CST6
|SystemV/CST6CDT
|US/Central
|US/Indiana-Starke
|America/Atikokan
|America/Bogota
|America/Cancun
|America/Cayman
|America/Coral_Harbour
|America/Detroit
|America/Eirunepe
|America/Fort_Wayne
|America/Guayaquil
|America/Havana
|America/Indiana/Indianapolis
|America/Indiana/Marengo
|America/Indiana/Petersburg
|America/Indiana/Vevay
|America/Indiana/Vincennes
|America/Indiana/Winamac
|America/Indianapolis
|America/Iqaluit
|America/Jamaica
|America/Kentucky/Louisville
|America/Kentucky/Monticello
|America/Lima
|America/Louisville
|America/Montreal
|America/Nassau
|America/New_York
|America/Nipigon
|America/Panama
|America/Pangnirtung
|America/Port-au-Prince
|America/Porto_Acre
|America/Rio_Branco
|America/Thunder_Bay
|America/Toronto
|Brazil/Acre
|Canada/Eastern
|Cuba
|EST
|EST5EDT
|Etc/GMT+5
|IET
|Jamaica
|SystemV/EST5
|SystemV/EST5EDT
|US/East-Indiana
|US/Eastern
|US/Michigan
|America/Caracas
|America/Anguilla
|America/Antigua
|America/Aruba
|America/Asuncion
|America/Barbados
|America/Blanc-Sablon
|America/Boa_Vista
|America/Campo_Grande
|America/Cuiaba
|America/Curacao
|America/Dominica
|America/Glace_Bay
|America/Goose_Bay
|America/Grenada
|America/Guadeloupe
|America/Guyana
|America/Halifax
|America/Kralendijk
|America/La_Paz
|America/Lower_Princes
|America/Manaus
|America/Marigot
|America/Martinique
|America/Moncton
|America/Montserrat
|America/Port_of_Spain
|America/Porto_Velho
|America/Puerto_Rico
|America/Santo_Domingo
|America/St_Barthelemy
|America/St_Kitts
|America/St_Lucia
|America/St_Thomas
|America/St_Vincent
|America/Thule
|America/Tortola
|America/Virgin
|Atlantic/Bermuda
|Brazil/West
|Canada/Atlantic
|Etc/GMT+4
|PRT
|SystemV/AST4
|SystemV/AST4ADT
|America/St_Johns
|CNT
|Canada/Newfoundland
|AGT
|America/Araguaina
|America/Argentina/Buenos_Aires
|America/Argentina/Catamarca
|America/Argentina/ComodRivadavia
|America/Argentina/Cordoba
|America/Argentina/Jujuy
|America/Argentina/La_Rioja
|America/Argentina/Mendoza
|America/Argentina/Rio_Gallegos
|America/Argentina/Salta
|America/Argentina/San_Juan
|America/Argentina/San_Luis
|America/Argentina/Tucuman
|America/Argentina/Ushuaia
|America/Bahia
|America/Belem
|America/Buenos_Aires
|America/Catamarca
|America/Cayenne
|America/Cordoba
|America/Fortaleza
|America/Godthab
|America/Jujuy
|America/Maceio
|America/Mendoza
|America/Miquelon
|America/Montevideo
|America/Paramaribo
|America/Recife
|America/Rosario
|America/Santarem
|America/Sao_Paulo
|Antarctica/Rothera
|Atlantic/Stanley
|BET
|Brazil/East
|Etc/GMT+3
|America/Noronha
|Atlantic/South_Georgia
|Brazil/DeNoronha
|Etc/GMT+2
|America/Scoresbysund
|Atlantic/Azores
|Atlantic/Cape_Verde
|Etc/GMT+1
|Africa/Abidjan
|Africa/Accra
|Africa/Bamako
|Africa/Banjul
|Africa/Bissau
|Africa/Casablanca
|Africa/Conakry
|Africa/Dakar
|Africa/El_Aaiun
|Africa/Freetown
|Africa/Lome
|Africa/Monrovia
|Africa/Nouakchott
|Africa/Ouagadougou
|Africa/Sao_Tome
|Africa/Timbuktu
|America/Danmarkshavn
|Antarctica/Troll
|Atlantic/Canary
|Atlantic/Faeroe
|Atlantic/Faroe
|Atlantic/Madeira
|Atlantic/Reykjavik
|Atlantic/St_Helena
|Eire
|Etc/GMT
|Etc/GMT+0
|Etc/GMT-0
|Etc/GMT0
|Etc/Greenwich
|Etc/UCT
|Etc/UTC
|Etc/Universal
|Etc/Zulu
|Europe/Belfast
|Europe/Dublin
|Europe/Guernsey
|Europe/Isle_of_Man
|Europe/Jersey
|Europe/Lisbon
|Europe/London
|GB
|GB-Eire
|GMT
|GMT0
|Greenwich
|Iceland
|Portugal
|UCT
|UTC
|Universal
|WET
|Zulu
|Africa/Algiers
|Africa/Bangui
|Africa/Brazzaville
|Africa/Ceuta
|Africa/Douala
|Africa/Kinshasa
|Africa/Lagos
|Africa/Libreville
|Africa/Luanda
|Africa/Malabo
|Africa/Ndjamena
|Africa/Niamey
|Africa/Porto-Novo
|Africa/Tunis
|Africa/Windhoek
|Arctic/Longyearbyen
|Atlantic/Jan_Mayen
|CET
|ECT
|Etc/GMT-1
|Europe/Amsterdam
|Europe/Andorra
|Europe/Belgrade
|Europe/Berlin
|Europe/Bratislava
|Europe/Brussels
|Europe/Budapest
|Europe/Busingen
|Europe/Copenhagen
|Europe/Gibraltar
|Europe/Ljubljana
|Europe/Luxembourg
|Europe/Madrid
|Europe/Malta
|Europe/Monaco
|Europe/Oslo
|Europe/Paris
|Europe/Podgorica
|Europe/Prague
|Europe/Rome
|Europe/San_Marino
|Europe/Sarajevo
|Europe/Skopje
|Europe/Stockholm
|Europe/Tirane
|Europe/Vaduz
|Europe/Vatican
|Europe/Vienna
|Europe/Warsaw
|Europe/Zagreb
|Europe/Zurich
|MET
|Poland
|ART
|Africa/Blantyre
|Africa/Bujumbura
|Africa/Cairo
|Africa/Gaborone
|Africa/Harare
|Africa/Johannesburg
|Africa/Kigali
|Africa/Lubumbashi
|Africa/Lusaka
|Africa/Maputo
|Africa/Maseru
|Africa/Mbabane
|Africa/Tripoli
|Asia/Amman
|Asia/Beirut
|Asia/Damascus
|Asia/Gaza
|Asia/Hebron
|Asia/Istanbul
|Asia/Jerusalem
|Asia/Nicosia
|Asia/Tel_Aviv
|CAT
|EET
|Egypt
|Etc/GMT-2
|Europe/Athens
|Europe/Bucharest
|Europe/Chisinau
|Europe/Helsinki
|Europe/Istanbul
|Europe/Kaliningrad
|Europe/Kiev
|Europe/Mariehamn
|Europe/Nicosia
|Europe/Riga
|Europe/Sofia
|Europe/Tallinn
|Europe/Tiraspol
|Europe/Uzhgorod
|Europe/Vilnius
|Europe/Zaporozhye
|Israel
|Libya
|Turkey
|Africa/Addis_Ababa
|Africa/Asmara
|Africa/Asmera
|Africa/Dar_es_Salaam
|Africa/Djibouti
|Africa/Juba
|Africa/Kampala
|Africa/Khartoum
|Africa/Mogadishu
|Africa/Nairobi
|Antarctica/Syowa
|Asia/Aden
|Asia/Baghdad
|Asia/Bahrain
|Asia/Kuwait
|Asia/Qatar
|Asia/Riyadh
|EAT
|Etc/GMT-3
|Europe/Minsk
|Europe/Moscow
|Europe/Simferopol
|Europe/Volgograd
|Indian/Antananarivo
|Indian/Comoro
|Indian/Mayotte
|W-SU
|Asia/Riyadh87
|Asia/Riyadh88
|Asia/Riyadh89
|Mideast/Riyadh87
|Mideast/Riyadh88
|Mideast/Riyadh89
|Asia/Tehran
|Iran
|Asia/Baku
|Asia/Dubai
|Asia/Muscat
|Asia/Tbilisi
|Asia/Yerevan
|Etc/GMT-4
|Europe/Samara
|Indian/Mahe
|Indian/Mauritius
|Indian/Reunion
|NET
|Asia/Kabul
|Antarctica/Mawson
|Asia/Aqtau
|Asia/Aqtobe
|Asia/Ashgabat
|Asia/Ashkhabad
|Asia/Dushanbe
|Asia/Karachi
|Asia/Oral
|Asia/Samarkand
|Asia/Tashkent
|Asia/Yekaterinburg
|Etc/GMT-5
|Indian/Kerguelen
|Indian/Maldives
|PLT
|Asia/Calcutta
|Asia/Colombo
|Asia/Kolkata
|IST
|Asia/Kathmandu
|Asia/Katmandu
|Antarctica/Vostok
|Asia/Almaty
|Asia/Bishkek
|Asia/Dacca
|Asia/Dhaka
|Asia/Kashgar
|Asia/Novosibirsk
|Asia/Omsk
|Asia/Qyzylorda
|Asia/Thimbu
|Asia/Thimphu
|Asia/Urumqi
|BST
|Etc/GMT-6
|Indian/Chagos
|Asia/Rangoon
|Indian/Cocos
|Antarctica/Davis
|Asia/Bangkok
|Asia/Ho_Chi_Minh
|Asia/Hovd
|Asia/Jakarta
|Asia/Krasnoyarsk
|Asia/Novokuznetsk
|Asia/Phnom_Penh
|Asia/Pontianak
|Asia/Saigon
|Asia/Vientiane
|Etc/GMT-7
|Indian/Christmas
|VST
|Antarctica/Casey
|Asia/Brunei
|Asia/Chita
|Asia/Choibalsan
|Asia/Chongqing
|Asia/Chungking
|Asia/Harbin
|Asia/Hong_Kong
|Asia/Irkutsk
|Asia/Kuala_Lumpur
|Asia/Kuching
|Asia/Macao
|Asia/Macau
|Asia/Makassar
|Asia/Manila
|Asia/Shanghai
|Asia/Singapore
|Asia/Taipei
|Asia/Ujung_Pandang
|Asia/Ulaanbaatar
|Asia/Ulan_Bator
|Australia/Perth
|Australia/West
|CTT
|Etc/GMT-8
|Hongkong
|PRC
|Singapore
|Australia/Eucla
|Asia/Dili
|Asia/Jayapura
|Asia/Khandyga
|Asia/Pyongyang
|Asia/Seoul
|Asia/Tokyo
|Asia/Yakutsk
|Etc/GMT-9
|JST
|Japan
|Pacific/Palau
|ROK
|ACT
|Australia/Adelaide
|Australia/Broken_Hill
|Australia/Darwin
|Australia/North
|Australia/South
|Australia/Yancowinna
|AET
|Antarctica/DumontDUrville
|Asia/Magadan
|Asia/Sakhalin
|Asia/Ust-Nera
|Asia/Vladivostok
|Australia/ACT
|Australia/Brisbane
|Australia/Canberra
|Australia/Currie
|Australia/Hobart
|Australia/Lindeman
|Australia/Melbourne
|Australia/NSW
|Australia/Queensland
|Australia/Sydney
|Australia/Tasmania
|Australia/Victoria
|Etc/GMT-10
|Pacific/Chuuk
|Pacific/Guam
|Pacific/Port_Moresby
|Pacific/Saipan
|Pacific/Truk
|Pacific/Yap
|Australia/LHI
|Australia/Lord_Howe
|Antarctica/Macquarie
|Asia/Srednekolymsk
|Etc/GMT-11
|Pacific/Bougainville
|Pacific/Efate
|Pacific/Guadalcanal
|Pacific/Kosrae
|Pacific/Noumea
|Pacific/Pohnpei
|Pacific/Ponape
|SST
|Pacific/Norfolk
|Antarctica/McMurdo
|Antarctica/South_Pole
|Asia/Anadyr
|Asia/Kamchatka
|Etc/GMT-12
|Kwajalein
|NST
|NZ
|Pacific/Auckland
|Pacific/Fiji
|Pacific/Funafuti
|Pacific/Kwajalein
|Pacific/Majuro
|Pacific/Nauru
|Pacific/Tarawa
|Pacific/Wake
|Pacific/Wallis
|NZ-CHAT
|Pacific/Chatham
|Etc/GMT-13
|MIT
|Pacific/Apia
|Pacific/Enderbury
|Pacific/Fakaofo
|Pacific/Tongatapu
|Etc/GMT-14
|Pacific/Kiritimati
|Antarctica/Palmer
|America/Grand_Turk
|America/Santiago
|Pacific/Easter
|Chile/EasterIsland
|Chile/Continental



[^-^]:常用图片注释