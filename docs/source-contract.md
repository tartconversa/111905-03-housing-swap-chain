# 来源资料约定

匿名交易、关联授权凭证和分段资金去向。

本渠道键以channel加local_id区分，不能跨渠道直接相等连接；授权凭证kind为household或fund_path，二者证明内容不同。

scope_project与valid_from/valid_to共同限定凭证，revokes引用原凭证，撤销自本条valid_from业务期生效，received表示合作方获知时间。歧义凭证给出多个候选，不声明唯一家庭。

funds金额为人民币分，release可归属资金、return回退原release的资金；allocations只是来源声称的使用意向，actual表示已核实用途凭证，不构成无限额度。

交易kind为sale/purchase，stage可为intent/signed/settled；完成出售以settled为准，完成购买以signed或settled为准。回流指从购买再经过卖出指向本链已经使用的资金来源，不创造额外本金。

## 数据载体

每类资料位于 data 中同名的 NDJSON 文件，一行一个原始对象；字段和基本类型见 data/schema.json。data/manifest.json 固定资料行数与字节哈希。资料中的缺失、歧义、未完整交付及冲突声明属于来源事实，不由资料检查程序推断业务结论。

### transactions

各渠道交易事实。字段：channel (string)、local_id (string)、kind (string)、stage (string)、project (string)、date (string)、amount_cents (integer)。

### credentials

受限关联凭证。字段：credential_id (string)、kind (string)、left (object)、candidates (array)、scope_project (string)、valid_from (string)、valid_to (any)、received (string)、revokes (any)。

### funds

实际释放与退回。字段：fund_id (string)、sale_channel (string)、sale_id (string)、kind (string)、amount_cents (integer)、date (string)、reference (any)。

### allocations

有凭证的资金使用片段。字段：piece_id (string)、fund_id (string)、purchase (string)、amount_cents (integer)、credential (string)、actual (boolean)。

