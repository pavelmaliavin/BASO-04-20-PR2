Используя intitle:"forum" inurl:http after:2019, я взял для примера http://www.forumarctic.com

IP определил в консоли с помощью команд ping, nslookup и немного костыльно через wireshark (можно было через сторонние ресурсы, но это и так очевидно)

<a href="https://ibb.co/9chrm85"><img src="https://i.ibb.co/DzYRPV2/image.png" alt="image" border="0"></a>

<a href="https://ibb.co/q1xH5Pp"><img src="https://i.ibb.co/PCt2Qfg/image.png" alt="image" border="0"></a>

<a href="https://ibb.co/VpZFtdK"><img src="https://i.ibb.co/n8vXbTW/image.png" alt="image" border="0"></a>

Далее в коммандной строке вбиваю nslookup с доп.опциями -query=mx, soa, nx; type=any.

1)-query=mx

С помощью данной опции я увидел, что forumarctic.com имеет только один почтовый сервер mx.yandex.net, также указан приоритет, но он не имеет значения, так как он не имееет резервного почтового сервера.

<a href="https://ibb.co/HCWxfXd"><img src="https://i.ibb.co/kJR9sM1/image.png" alt="image" border="0"></a>

2)-type=any

Здесь находятся все записи о DNS для нашего домена, например название сервера и т.д 

<a href="https://ibb.co/rsbvbBk"><img src="https://i.ibb.co/5FY2YQT/image.png" alt="image" border="0"></a>

3)-query=soa

Вся инфа о доменной зоне, тут и так все понятно

<a href="https://ibb.co/5npsMyR"><img src="https://i.ibb.co/b2wNWZK/image.png" alt="image" border="0"></a>

4)-query=nx

Никакой разницы между nslookup и nslookup -query=nx я не увидел, если честно, насчет nx вообще ничего не нашел

<a href="https://ibb.co/L18HFP0"><img src="https://i.ibb.co/yqX79RN/image.png" alt="image" border="0"></a>

Теперь с помощью wireshark будем перехватывать и изучать трафик с нашего сайта

Для этого мы используем дисплейные фильтры ip.src==(((айпинейм))), ip.dst;

ip.addr, udp.src, arp.src.hw_mac, eth.dst, eth.src

1)ip.src=="ip" перехватывает трафик прешедшего с айпи (source -> destination)

<a href="https://ibb.co/x6TjVzS"><img src="https://i.ibb.co/rHr6n5G/image.png" alt="image" border="0"></a>

2)ip.dst=="ip" тоже самое, только наоборот, перехватывает трафик от моего айпи до айпи сайта (destination -> source)

<a href="https://ibb.co/kx88GtP"><img src="https://i.ibb.co/7XWWSq0/image.png" alt="image" border="0"></a>

Анализируя с помощью этих фильтров, можно понять, какие протоколы использует сайт (HTTP / TCP), какие порты используются для подключения к нему (80, стандарнтный для сайтов), да и можно взять любой пакет трафика по определенному айпи и посмотреть всю информацию.

Можно заметить, что при активности на сайте пакеты имеет протокол HTTP, но также имеются пакеты с протоколом TCP, в котором, как я понял, можно увидеть из какого в какой порт идет пакет (в зависимости от использования фильтра ip.src либо ip.dst) , а также дополнительная информация

3)ip.addr вроде бы комбинирует фильтры ip.src и ip.dst, а то есть фильрует именно по айпи все пакеты, связанные с ним.

<a href="https://ibb.co/RcXZrWM"><img src="https://i.ibb.co/SBkWYp1/image.png" alt="image" border="0"></a>

4)udp.src не заработала вообще, пришлось гуглить. Нашел похожую команду udp.srcport, который фильтрует по UDP порту (не очень понял, что это такое, но как говорится, очень интересно)

<a href="https://ibb.co/52W3rsK"><img src="https://i.ibb.co/mSCK9hN/image.png" alt="image" border="0"></a>

5)arp.src.hw_mac как фильтр по протоколу arp, только с доп.фильтрацией по мак (если его указать, конечно).

<a href="https://ibb.co/X8WH1R0"><img src="https://i.ibb.co/cLyq0nf/image.png" alt="image" border="0"></a>

6)eth.src/dst фильтрует все пакеты по маку. Если его не указывать, то будет выдавать вообще все пакеты по всем протоколам (вроде бы)

<a href="https://ibb.co/PjtnnZC"><img src="https://i.ibb.co/g96ppMg/image.png" alt="image" border="0"></a>

<a href="https://ibb.co/0Kbhmwz"><img src="https://i.ibb.co/DY25zqj/image.png" alt="image" border="0"></a>

Это было последнее задание практической работы 2.
Дополнительные источники при выполнении работы https://hackware.ru/?p=7008


<a href="https://imgbb.com/"><img src="https://i.ibb.co/WV3bk5C/image.png" alt="image" border="0"></a>
