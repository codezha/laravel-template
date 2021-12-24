``` shell script
# 验证
php artisan make:request StoreQuestionRequest
# 模型
php artisan make:model Admin/Setting
# 数据库
php artisan make:migration create_accounts_tables --create=accounts
# 控制器
php artisan make:controller Admin/Mes/Base/OutgoingDispatchSettingController --resource
# 生成数据
php artisan migrate
# 刷新数据并填充
php artisan migrate:refresh --seed
# 创建填充数据
php artisan make:seeder UsersTableSeeder
# 生成填充数据
php artisan db:seed  --class=ProductTableSeeder
# 查看所有路由
php artisan route:list
# 自定义验证
php artisan make:rule ValidateRegisterMobileRule
# 监听邮件
http://localhost:8025
# 缓存配置
php artisan config:cache
php artisan config:clear
# 缓存路由
php artisan route:cache
php artisan route:clear
# 缓存视图
php artisan view:cache
php artisan view:clear
# 缓存事件
php artisan event:cache
php artisan event:clear
# 清除所有缓存 页面 redis 配置文件
php artisan cache:clear
# 监听指定队列
php artisan queue:work --queue=download_jlpay_data,import_jlpay_activation
# 维护模式
php artisan down --retry=60
# 绕过维护模式
php artisan down --secret="1630542a-246b-4b66-afa1-dd72a4c43515"
# 维护模式视图
php artisan down --render="errors::503"
# 重定向维护模式
php artisan down --redirect=/
# 关闭维护模式
php artisan up
# 0停机部署解决方案
[envoyer](https://envoyer.io/)
[vapor](https://vapor.laravel.com/)









```

