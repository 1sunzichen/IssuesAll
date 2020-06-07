* 1 保留2位小数   formatter  parser 属性设置一下
```const limitDecimals = (value) => {
        const reg = /^(\-)*(\d+)\.(\d\d).*$/;
        console.log(value);
        if(typeof value === 'string') {
            return !isNaN(Number(value)) ? value.replace(reg, '$1$2.$3') : ''
        } else if (typeof value === 'number') {
            return !isNaN(value) ? String(value).replace(reg, '$1$2.$3') : ''
        } else {
            return ''
        }
    }
```
```
<Item
  label="储值卡订单"
  name="rateValueCard"
>
    <InputNumber placeholder="请输入" max={0.3} step={0.01} 
    formatter={limitDecimals}
    parser={limitDecimals}
    defaultValue={0} min={0.0} 
     // disabled={!currentValue}
/>
</Item>
```
