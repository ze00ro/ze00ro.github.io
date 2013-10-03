##灵动创展电影票电子兑换券接口文档

###接口规范
1. 业务平台统一使用 `UTF-8` 编码方式。
2. 参数名称和参数说明中规定的固定值须与列表中一致，**大小写敏感**。
3. 渠道 `partner_id` 是业务平台分配给每个渠道唯一的渠道号。
4. 渠道密钥是由灵动创展提供的业务管理平台分配给每个渠道唯一的渠道密码，是一个 `MD5` 码，**需要严格保密**。
5. 每个接口的的调用权限不同，调用权限由灵动创展提供。
6. 每个接口的输入参数采用 `HTTP POST` 方式传递。接口前7个参数相同为 `partner_id,sec_id,bus_id,req_id,ver,timestamp` 后面紧跟每个接口的业务所需参数, 最后为 `hashcode` 参数
7. 同一业务的接口会存放在同一目录下，且接口名为调用功能的接口名，每个接口的接口详细参数参见后面章节。

调用接口的例子如下, 具体代码请参见demo, 参数说明请参见下文

    POST http://api.example.com/apiname/apimethod/...
    POST data
        partner_id=CS000001
        sec_id=test
        bus_id=0102
        req_id=2012010100000001
        ver=1000&
        timestamp=2012030109241825
        hashcode=2342asdfdfade3asf2342

###平台接口参数说明
<table>			
<thead>
<th>参数</th>
<th>说明</th>
<th>可空</th>
<th>举例</th>
</thead>
<tbody>
<tr>
<td>partner_id</td>
<td>合作方ID,由两位大写字母缩写加6位数字码共8位组成(由平台分配)</td>
<td>N</td>
<td>灵动提供的合作ID, 如CS000001</td>
</tr>
<tr>
<td>bus_id</td>
<td>业务ID,为7位数字码（由平台分配）</td>
<td>N</td>
<td>灵动提供的业务ID, 如1002758</td>
</tr>
<tr>
<td>sec_id</td>
<td>安全配置ID（由平台分配）</td>
<td>N</td>
<td>灵动提供的安全配置ID, 如Test</td>
</tr>
<tr>
<td>req_id</td>
<td>请求流水号（请求消息ID）,20位以下, 只能为字母数字</td>
<td>N</td>
<td>不可重复, 如2012010100000001</td>
</tr>
<tr>
<td>ver</td>
<td>协议版本采用4位数字，主版本 (1 位) 次版本 (1 位) 修订版本 (2 位)</td>
<td>N</td>
<td>如1000</td>
</tr>
<tr>
<td>timestamp</td>
<td>时间戳：如没有特殊说明，格式均按照：YmdHis格式</td>
<td>N</td>
<td>年月日时分秒16位, 如20121225020202</td>
</tr>
<tr>
<td>hashcode</td>
<td>以上各项参数按顺序外加灵动提供的md5key组成字符串后md5后的值</td>
<td>N</td>
<td>md5(partner_id+bus_id+req_id+timestamp+md5key)</td>
</tr>
</tbody>
</table>

###返回参数说明
XML格式的成功返回

    <?xml version="1.0" encoding="UTF-8"?>
    <response>
        <code>200</code>
        <data>
            ...
        </data>
    </response>

JSON格式的成功返回

    {
    "code":200,
    "data":{...},
    "param":其他一些接口的特定参数
    }

XML格式的错误返回

    <?xml version="1.0" encoding="UTF-8"?>
    <response>
        <code>错误码</code>
        <error>
            <message>错误描述</message>
            <type>错误类型</type>
        </error>
    </response>

JSON格式的错误返回

    {
    "code":错误码,
    "error":{"message":"错误描述","type":"错误类型"}
    }

###错误码介绍
错误码遵循HTTP状态码, 具体参见 [RFC2616][]
2xx均为成功, 如接口无特殊说明, 均可只以200判断成功

- 200 成功
- 201 创建成功
- 202 接受到请求, 会异步处理
- 204 没有数据
- 302 
- 304 
- 400 请求参数有误
- 401 没有权限
- 403 禁止访问
- 404 找不到请求的接口
- 405 不支持请求的方法, 现在只支持POST请求
- 406 请求无法完成
- 410 接口已删除, 请使用新接口
- 500 服务器内部处理错误
- 503 服务器端暂时无法处理请求（可能是过载或维护）
- 504 超时

###接口列表
购票有两种调用流程按影院购买(1,2,3,4,7)或按卡类购买(5,6,7), 1,2,5,6由于变化不大都可以采取定时更新的方式

1. 获取支持的城市信息 [movTicket/cities](#cities)
2. 通过城市获取支持的区域信息 [movTicket/regionsByCity](#regionsByCity)
3. 通过区域获得支持的影院信息与对应的价格 [movTicket/cinemasByRegion](#cinemasByRegion)
4. 按影院生成一个卡号和需要支付的总价(卡为未激活状态) [movTicket/buyByCinema](#buyByCinema)
5. 获取卡的种类 [movTicket/kinds](#kinds)
6. 获取卡类支持的影院 [movTicket/cinemasByKind](#cinemasByKind)
7. 按卡类生成一个卡号和需要支付的总价(卡为未激活状态) [movTicket/buyByKind](#buyByKind)
8. 按卡号激活卡并发送短信(当用户支付完后需要调用的接口) [movTicket/payByCard](#payByCard)
9. 按卡号查询卡的状态(暂不可用) [movTicket/status](#status)
10. 按卡号作废一张卡(暂不可用) [movTicket/ban](#ban)

<div id="cities"></div>
####获取支持的城市信息 movTicket/cities
接口参数 无接口参数

结果示例 XML

    <response>
        <code>200</code>
        <data>
        <cities>
            <item>
                <code_id>001</code_id>
                <parent_id>0</parent_id>
                <code_name>北京</code_name>
                <remark>bj</remark>
            </item>
            <item>
                <code_id>002</code_id>
                <parent_id>0</parent_id>
                <code_name>武汉</code_name>
                <remark>wh</remark>
            </item>
            ...
        </cities>
        </data>
    </response>

结果示例 JSON

    {"code":200,"data":
       {"cities":[
          {"code_id":"001","parent_id":"0","code_name":"\u5317\u4eac","remark":"bj"},{"code_id":"002","parent_id":"0","code_name":"\u6b66\u6c49","remark":"wh"},
          ...
       ]}
    }

<div id="regionsByCity"></div>
####通过城市获取支持的区域信息 movTicket/regionsByCity
接口参数
<table>
<thead>
<th>参数</th>
<th>说明</th>
<th>可空</th>
<th>举例</th>
</thead>
<tbody>
<tr>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</tbody>
</table>
结果示例

<div id="cinemasByRegion"></div>
####通过区域获得支持的影院信息与对应的价格 movTicket/cinemasByRegion

<div id="buyByCinema"></div>
####按影院生成一个卡号和需要支付的总价 movTicket/buyByCinema

<div id="kinds"></div>
####获取卡的种类 movTicket/kinds

<div id="cinemasByKind"></div>
####获取卡类支持的影院 movTicket/cinemasByKind

<div id="buyByKind"></div>
####按卡类生成一个卡号和需要支付的总价 movTicket/buyByKind

<div id="payByCard"></div>
####按卡号激活卡并发送短信 movTicket/payByCard

<div id="status"></div>
####按卡号查询卡的状态 movTicket/status

<div id="ban"></div>
####按卡号作废一张卡 movTicket/ban


[RFC2616]:	http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html	"RFC2616"