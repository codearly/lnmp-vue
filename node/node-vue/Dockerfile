# nodejs基础镜像，这里可以使用其它tag，alpine 镜像比较小，适合用在虚拟机上
FROM node:lts-alpine

# 导出8000和8080端口分别是vue ui的web管理页面端口以及serve端口
EXPOSE 8000
EXPOSE 8080
WORKDIR /usr/src

# 如果使用npm则改成 npm install -g @vue/cli
RUN yarn global add @vue/cli
# 这里因为在vue ui中默认hosts是localhost，所以不能通过外部访问，这里需要将host修改为0.0.0.0，才能外部访问
CMD ["vue", "ui", "--host", "0.0.0.0"]

LABEL Author="Liu"
LABEL Version="1.0-vue@cli"
LABEL Description="vue@cli 镜像."

# 这里需要注意vue ui以及serve的端口默认的host都是不能通过外部访问的需要修改配置文件,vue.config.js需要添加devServer将host设置为0.0.0.0，vue ui部分可以使用–host参数设置

#--------vue.config.js-----------
# module.exports = {
# publicPath: '/',

# devServer:{
#     host: '0.0.0.0',
#     port: 8080,
# },
# productionSourceMap: false,
# lintOnSave: undefined
# }
#--------------------------------