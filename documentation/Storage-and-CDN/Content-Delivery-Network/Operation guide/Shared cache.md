# 共享缓存

适用于多个域名共享一个主域名的缓存策略，可将这些域名添加在一个分组内并开启共享缓存功能，即组内域名将共享同一份缓存策略，从而降低回源。如域名a.com和b.com共享a.com的缓存策略，设置共享缓存后，用户请求b.com时返回a.com的缓存策略。

1、登录CDN控制台，打开域名管理，选择“域名组列表”，点击“添加域名组”进行新建域名组。

2、新建域名组，可选择需要共享缓存的域名列表，并设置要共享哪个域名的缓存策略，即为主域名。
泛域名作不建立域名组时也可共享缓存，即泛域名为*.a.com，在“缓存配置”中可单独打开共享缓存配置，即*.a.com下的所有子域名均请求*.a.com下的缓存策略，泛域名也可作为独立的域名与其他域名建立域名组共享缓存。
