# Ubox

友宝开放api

## Status
代码 已经打包发布到 RubyGems.

[![image](https://ruby-china-files.b0.upaiyun.com/photo/5982eaaa64f467d9dbda03ad4f40ea27.png)](https://rubygems.org/gems/ubox)

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'ubox'
```

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install ubox

## Config
```ruby
  Ubox.configure do |ubox|
    ubox.app_id = 'your_app_id'
    ubox.app_key = 'your_app_key' 
    ubox.api_url = 'http://uboxapi.dev.uboxol.com/opentrade' #正式环境请用正式环境url
  end
```

## Usage

###getProductDetail 获取商品信息(扫码)
```ruby
detail=Ubox.product_detail(qr_string:'http://v.dev.uboxol.com/qr/c0081801_10000870_1')
```

```json
{
 "head": {
      "return_code": 200,
      "return_msg": "正常响应"
 },
 "body": {
     "vm": {
       "id": "0081801",
       "name": "友宝在线六层富二代完鼎测试需要改动名称完鼎测试需要改动名称完",
       "address": "北京111111",
       "distance": "",
       "is_shop": false,
       "has_box": false,
       "has_meal": false,
       "lat": "116.480557",
       "lng": "39.960201",
       "vm_connected": true
    },
    "product": {
          "id": 10000870,
          "name": "双层咖啡杯",
          "oldPrice": 200,
          "price": 200,
          "expire_time": 1464856997,
          "pic": "http://vms.dev.uboxol.com/upload/box-tmp/m/10000870/10000870.jpg?t=1357177524",
          "num": 16
   },
   "tran_id": "100031290"
 }
}
```
### notifyOrder 扫码下单请求
```ruby
notify = Ubox.notify_order(tran_id: detail['body']['tran_id'],
                           retain_period: 300,
                           app_tran_id: Random.rand_str(32),
                           app_uid: Random.rand_str(32)
     )
```

```json
{"head":{
    "return_code":200, 
    "return_msg":"正常响应"
  }
}
```

### notifyPament 买取货码(支付结果通知)

```ruby
payment = Ubox.notify_payment(tran_id: detail['body']['tran_id'],
                                  pay_time: Time.now.to_i,
                                  app_current_time: Time.now.to_i,
                                  deliver_now: true)
```
#### 商品已售空
```json
{"head":{
    "return_code":410, 
    "return_msg":"该商品已售空"
 }
}
```

### getTradeOrderStatus 出货结果询问 

```ruby
status = Ubox.trade_order_status(tran_id: detail['body']['tran_id'])
```
#### 出货失败
```json
{"head":{
    "return_code":200, 
    "return_msg":"正常响应"
 }, 
 "body":{
    "code":500, 
    "msg":"出货失败"
 }
}
```

### searchVmList 搜索售货机接口

```ruby
list = Ubox.search_vm_list(keyword:'北京香颂')
```

```json

```

## Development

After checking out the repo, run `bin/setup` to install dependencies. Then, run `rake spec` to run the tests. You can also run `bin/console` for an interactive prompt that will allow you to experiment.

To install this gem onto your local machine, run `bundle exec rake install`. To release a new version, update the version number in `version.rb`, and then run `bundle exec rake release`, which will create a git tag for the version, push git commits and tags, and push the `.gem` file to [rubygems.org](https://rubygems.org).

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/[USERNAME]/ubox. This project is intended to be a safe, welcoming space for collaboration, and contributors are expected to adhere to the [Contributor Covenant](http://contributor-covenant.org) code of conduct.


## License

The gem is available as open source under the terms of the [MIT License](http://opensource.org/licenses/MIT).

