
**更新时间**：{docsify-updated} 

## 国际化处理
对接口响应返回的retCode和retInfo不做国际化处理，由接口调用方处理。
对于接口涉及业务数据的国际化通过在header中传递language参数来定义，具体的国际化语言代码见下。

|语言编码|	英文名称|	中文名称|	是否支持|
|-----|----|----|----|
|af	|Afrikaans - South Africa	|南非荷兰语|	否|
|ar-ae	|Arabic(U.A.E.)	|阿拉伯语 - 阿拉伯联合酋长国	|否|
|ar-bh	|Arabic(Bahrain)	|阿拉伯语 - 巴林	|否|
|ar-dz	|Arabic(Algeria)	|阿拉伯语 - 阿尔及利亚	|否|
|ar-eg	|Arabic(Egypt)	|阿拉伯语 - 埃及	|否|
|ar-iq	|Arabic(Iraq)	|阿拉伯语 - 伊拉克	|否|
|ar-jo	|Arabic(Jordan)	|阿拉伯语 - 约旦	|否|
|ar-kw	|Arabic(Kuwait)	|阿拉伯语 - 科威特	|否|
|ar-lb	|Arabic(Lebanon)	|阿拉伯语 - 黎巴嫩	|否|
|ar-ly	|Arabic(Libya)	|阿拉伯语 - 利比亚	|否|
|ar-ma	|Arabic(Morocco)	|阿拉伯语 - 摩洛哥	|否|
|ar-om	|Arabic(Oman)	|阿拉伯语 - 阿曼	|否|
|ar-qa	|Arabic(Qatar)	|阿拉伯语 - 卡塔尔	|否|
|ar-sa	|Arabic(Saudi Arabia)	|阿拉伯语 - 沙特阿拉伯	|否|
|ar-sy	|Arabic(Syria)	|阿拉伯语 - 叙利亚	|否|
|ar-tn	|Arabic(Tunisia)	|阿拉伯语 - 突尼斯	|否|
|ar-ye	|Arabic(Yemen)	|阿拉伯语 - 也门	|否|
|be	|Belarusian	|白俄罗斯语	|否|
|bg	|Bulgarian	|保加利亚语	|否|
|ca	|Catalan	|加泰罗尼亚语	|否|
|cs|	Czech	|捷克语|	否|
|da	|Danish	|丹麦语	|否|
|de	|German(Standard)	|德语 - 标准|	否|
|de-at	|German(Austrian)	|德语 - 奥地利	|否|
|de-ch	|German(Swiss)	|德语 - 瑞士|	否|
|de-li	|German(Liechtenstein)	|德语 - 列支敦士登|	否|
|de-lu	|German(Luxembourg)	|德语 - 卢森堡	|否|
|el	|Greek	|希腊语	|否|
|en	|English	|英语|	是|
|en-au	|English(Australian)	|英语 - 澳大利亚	|否|
|en-bz	|English(Belize)	|英语 - 伯利兹	|否|
|en-ca	|English(Canadian)	|英语 - 加拿大	|否|
|en-gb	|English(British)	|英语 - 英国	|否|
|en-ie	|English(Ireland)	|英语 - 爱尔兰|	否|
|en-jm	|English(Jamaica)	|英语 - 牙买加	|否|
|en-nz	|English(New Zealand)	|英语 - 新西兰	|否|
|en-tt	|English(Trinidad)	|英语 - 特立尼达岛	|否|
|en-us	|English(United States)	|英语 - 美国	|否|
|en-za	|English(South Africa)	|英语 - 南非	|否|
|es	|Spanish(Spain - Modern Sort)	|西班牙语 - 标准|	否|


## 安全规范

1. **数据安全**

App应用要保证自己接入UWS时通过海极网获取的appid、appkey等信息的使用安全，出现因外泄导致违规使用UWS服务而被平台禁用的风险由APP开发者自行承担。
App通过UWS获取的数据等要确保存储安全。

2. **接口访问安全**

要严格按照UWS接口服务的使用文档调用,开发相关安全参照“海尔优家安全开发规范V1.0.pdf”执行。


## 常见HTTP状态码

状态码|含义|解决思路
:-:|:-|:-
200|（成功）</br>请求已成功，请求所希望的响应头或数据体将随此响应返回|
400|（错误请求）</br>1、语义有误，当前请求无法被服务器理解。除非进行修改，否则客户端不应该重复提交这个请求。</br>2、请求参数有误。|对照接口文档，检查请求参数、方法等是否正确
401|（身份验证错误）</br>请求要求身份验证。 对于需要登录的网页，服务器可能返回此响应。|对照接口文档，检查请求参数、方法等是否正确
403|（禁止）</br>服务器已经理解请求，但是拒绝执行它。与401响应不同的是，身份验证并不能提供任何帮助，而且这个请求也不应该被重复提交。|对照接口文档，检查请求参数、方法等是否正确
404|（未找到）</br>请求失败，请求所希望得到的资源未被在服务器上发现。|对照接口文档，检查请求参数、方法等是否正确
500|（服务器内部错误）</br>服务器遇到了一个未曾预料的状况，导致了它无法完成对请求的处理。一般来说，这个问题都会在服务器的程序码出错时出现。|排查对应的服务端出错情况或异常
501|（尚未实施）</br>服务器不具备完成请求的功能。例如，当服务器无法识别请求方法时，服务器可能会返回此代码。 |排查对应的服务端出错情况或异常
502|（错误网关)</br>服务器作为网关或代理，从上游服务器收到了无效的响应。 |排查对应的服务端出错情况或异常
503|（服务不可用）</br>由于临时的服务器维护或者过载，服务器当前无法处理请求。|排查对应的服务端出错情况或异常
504|（网关超时）</br>作为网关或者代理工作的服务器尝试执行请求时，未能及时从上游服务器（URI标识出的服务器，例如HTTP、FTP、LDAP）或者辅助服务器（例如DNS）收到响应。|排查对应的服务端出错情况或异常


## 平台公共错误码
|retCode错误码|	retInfo描述|可尝试解决方法|
|---|---|---|  
|10000|登录成功，但密码安全等级提高，请修改密码|用户使用弱密码|
|00001|登录成功但未接受最新版本隐私协议|老账号未接受隐私条款的情况|
|A00001|	服务不可用                                    |联系云平台检查服务是否正常
|A00002|	网络异常                                      |联系云平台检查服务是否正常
|A00003|	访问或操作超时                                |联系云平台检查服务是否正常
|A00004|	系统内部错误                                  |联系云平台检查服务是否正常
|A00005|	数据库访问异常                                |联系云平台检查服务是否正常
|A00006|	未知异常                                      |联系云平台检查服务是否正常
|A00007|	邮件服务异常                                  |联系云平台检查服务是否正常
|A00008|	邮件发送失败                                  |联系云平台检查服务是否正常
|A00009|	邮件发送次数超限                              |检查必填参数是否为有效值（非空并且不能为空字符串）
|B00001|	缺少必填参数                                  |N/A
|B00002|	参数类型错误                                  |N/A
|B00003|	参数数值超出值域或不是枚举值                  |N/A
|B00004|	参数不符合规则要求                            |N/A
|B00006|	参数长度错误                                  |N/A
|B00007|	参数与接口定义不匹配                          |N/A
|C00001|	appId与appKey验证失败                         |N/A
|C00002|	appServer无访问授权                           |N/A
|C00003|	访问权限不足                                  |N/A
|C00004|	操作权限不足                                  |N/A
|C00005|	重复请求                                      |N/A
|C00006|	未知设备类型                                  |N/A
|C00007|	appId配置信息为空                             |N/A
|C00008|	appKey为空                                    |N/A
|D00001|	sign签名错误                                  |N/A
|D00002|	账号或密码错误                                |N/A
|D00003|	Token不存在，未通过token验证                  |N/A
|D00004|	Token已过期，未通过token验证。                |N/A
|D00005|	Token不是由此应用创建，未通过token验证。      |N/A
|D00006|	会话失效                                      |N/A
|D00007|	不是系统内部用户                              |N/A
|D00008|	用户不合法                                    |N/A
|D00009|	登录失败超限，需使用验证码登录                |N/A
|D00010|	账号被锁定                                    |N/A
|D00011|	帐号未激活                                    |N/A
|D00012|	账号已存在                                    |N/A
|D00013|	登录失败                                      |N/A
|D00015|	验证码错误                                    |N/A
|D00016|	你已退出或尚未登录                            |N/A
|D00017|	账户不存在                                    |N/A
|G20003|	无法获取到适配器/应用信息                     |N/A
|G20201|	获取用户绑定的设备列表失败                    |N/A
|G20202|	当前用户与该设备不匹配                        |N/A
|G20301|	设备信息查询失败                              |N/A
|20903	|主数据访问异常                                   |N/A
|G20904|	已超过绑定数量                                |一个用户只能绑定100个设备
|G20908|	安全校验未通过                                |重新配网
|G20910|	非安全设备校验失败                            |非安全设备绑定：1、设备必须在线2、设备上报版本信息时间不大于10分钟，即小于等于10分钟
|G23401|	设备未绑定                                    |N/A
|G23501|	用户绑定设备安全校验失败                      |N/A
|G24001|	设备品牌型号信息为空                          |N/A





## 国际时区说明

|时区 TimeZone ID|
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