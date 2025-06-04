### 项目SaaS化配置

用于解决项目适用于不同国家地区，同一项目支持不同地区关于币种及时区的默认配置

步骤一：

该配置存入多地区对应币种及电话号码区号

目录：`saasConfig/HK/index.ts`

```javascript
export default const HK_Config = () => {
    saasCurrency: 'HKD',
    saasPhoneAreaCode: '852',
    saasAreaName: 'HongKong',
    saasPhoneReg: '^\\d{8}$',
    saasPhoneLength: 8,
    saasAreaCodeList: [
      { id: '852', code: '+852（HongKong）' },
      { id: '65', code: '+65（Singapore）' },
      { id: '86', code: '+86（China）' },
      { id: '886', code: '+886（Taiwan）' },
      { id: '60', code: '+60（Malaysia）' },
    ],
  }
```

目录：`saasConfig/AU/index.ts`

```javascript
export default const AU_Config = () => {
    currency: 'AUD',
    phoneAreaCode: '61',
    areaName: 'Australia',
    phoneReg: '^\\d{11}$',
    phoneLength: 11,
    areaCodeList: [
      { id: '61', code: '+61（Australia）' },
      { id: '65', code: '+65（Singapore）' },
      { id: '86', code: '+86（China）' },
      { id: '886', code: '+886（Taiwan）' },
      { id: '60', code: '+60（Malaysia）' },
    ],
  }
```

目录：`saasConfig/JP/index.ts`

```javascript
export default const JP_Config = () => {
    currency: 'JPY',
    phoneAreaCode: '81',
    areaName: 'Japan',
    phoneReg: '^\\d{11}$',
    phoneLength: 11,
    areaCodeList: [
      { id: '81', code: '+81（Japan）' },
      { id: '65', code: '+65（Singapore）' },
      { id: '86', code: '+86（China）' },
      { id: '886', code: '+886（Taiwan）' },
      { id: '60', code: '+60（Malaysia）' },
    ],
  }
```

目录：`saasConfig/index.ts`

```react
const saasConfig = () =>{
    switch(saas_env){
        case 'HK':
        	return {...HK_Config};
        case 'AU:
        	return {...AU_Conifg};
        case 'JP:
        	return {...JP_Config};
        default:
            return {...HK_Config};
    }
}

export {saasCurrency,saasPhoneAreaCode,saasAreaName,saasPhoneReg,saasPhoneLength,saasAreaCodeList} = saasConfig()
```

步驟二：

在`config`目录的`start`文件下，引用`saas`配置项文件，并根据`webpack`配置的打包项显示不同地区对应配置显示

`config\config.start.ts`

```react
import config from './config.route';
import { defineConfig } from 'umi';
import sassConfig from './config.saas';

let $baseUrl = 'http://localhost:' + (process.env.PORT || '8000');
let $saas_env = process.env.SAAS_ENV || 'hk'

export default defineConfig(
  Object.assign({}, config, {
    define: {
      'process.env.baseUrl': $baseUrl,
      saas_env: $saas_env,
    },
  }),
);
```

步骤三：

在webpack的package.json的启动命令中传入当前处于哪个地区，进行配置化编译，显示对应时区编码及币种

:tipping_hand_man:主要代码：

`  "start:test-a-au": "pnpm origin-init && cross-env PORT=8006 SAAS_ENV=au UMI_ENV=start `

```json
{
  "name": "kpay-crm",
  "version": "1.0.0",
  "private": true,
  "description": "KPay CRM PC",
  "scripts": {
    "analyze": "cross-env ANALYZE=1 umi build",
    "origin-init": "npx kcli kp:command-chain && npx kapi",
    "start-yapi": "pnpm origin-init && cross-env PORT=8011 UMI_ENV=yapi MOCK=true REACT_APP_ENV=yapi UMI_UI=none umi dev",
    "tw-build": "pnpm origin-init && cross-env KPAY_BUILD_CLI=testA UMI_ENV=test.tw UMI_UI=none MOCK=none umi build",
    "start-a": "pnpm origin-init && cross-env PORT=8000 UMI_ENV=start REACT_APP_ENV=devA UMI_UI=none MOCK=none umi dev",
    "start-b": "pnpm origin-init && cross-env PORT=8001 UMI_ENV=start REACT_APP_ENV=devB UMI_UI=none MOCK=none umi dev",
    "start-c": "pnpm origin-init && cross-env PORT=8002 UMI_ENV=start REACT_APP_ENV=devC UMI_UI=none MOCK=none umi dev",
    "start-d": "pnpm origin-init && cross-env PORT=8012 UMI_ENV=start REACT_APP_ENV=devD UMI_UI=none MOCK=none umi dev",
    "start:test-a": "pnpm origin-init && cross-env PORT=8003 UMI_ENV=start REACT_APP_ENV=testA UMI_UI=none MOCK=none umi dev",
    "start:test-b": "pnpm origin-init && cross-env PORT=8004 UMI_ENV=start REACT_APP_ENV=testB UMI_UI=none MOCK=none umi dev",
    "start:test-c": "pnpm origin-init && cross-env PORT=8005 UMI_ENV=start REACT_APP_ENV=testC UMI_UI=none MOCK=none umi dev",
    "start:test-d": "pnpm origin-init && cross-env PORT=8008 UMI_ENV=start REACT_APP_ENV=testD UMI_UI=none MOCK=none umi dev",
    "start:test-a-au": "pnpm origin-init && cross-env PORT=8006 SAAS_ENV=au UMI_ENV=start REACT_APP_ENV=pre UMI_UI=none MOCK=none umi dev",
    "start:pre": "pnpm origin-init && cross-env PORT=8006 UMI_ENV=start REACT_APP_ENV=pre UMI_UI=none MOCK=none umi dev",
    "build-dev": "pnpm origin-init && cross-env UMI_ENV=beta.dev UMI_UI=none MOCK=none umi build",
    "build": "pnpm origin-init && cross-env UMI_ENV=beta UMI_UI=none MOCK=none NODE_OPTIONS=--max_old_space_size=5632  umi build",
    "stage": "pnpm origin-init && cross-env UMI_ENV=stage UMI_UI=none MOCK=none NODE_OPTIONS=--max_old_space_size=5632 umi build",
    "pro": "pnpm origin-init && cross-env UMI_ENV=pro UMI_UI=none MOCK=none umi build",
    "pre-prod": "pnpm origin-init && cross-env UMI_ENV=pre.prod UMI_UI=none MOCK=none umi build",
  }
```

步骤四：公共配置type.ts

```typescript
declare const SAAS_CONFIG: SaasConfig;

// google analytics interface
interface GAFieldsObject {
  eventCategory: string;
  eventAction: string;
  eventLabel?: string;
  eventValue?: number;
  nonInteraction?: boolean;
}
```

在项目中使用

```typescript
SAAS_CONFIG.currency//根据配置获取当前地区对应的币种
```
