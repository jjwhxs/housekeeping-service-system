### 系统介绍

基于SpringBoot和Vue实现的家政服务系统采用前后端分离的架构方式，系统设计了三种角色，分别是用户、家政人员、管理员，每种角色拥有不同的菜单权限，用户可以在前台系统中进行注册/登录、首页、家政人员、公告信息等功能模块操作，家政人员可以在后台系统中进行注册/登录、系统首页、个人中心、即时通讯管理、通讯回复管理、预约订单管理、接单信息管理、服务费用管理、服务评价管理等功能模块操作，管理员可以在后台系统中进行系统登录、系统首页、个人中心、用户管理、家政人员管理、服务类别管理、即时通讯管理、通讯回复管理、预约订单管理、接单信息管理、服务费用管理、服务评价管理、系统管理等功能模块操作。

### 技术选型

开发工具：idea2020.3+Webstorm2020.3

运行环境：jdk1.8+maven3.6.3+mysql5.7+nodejs12.19.0

服务端技术：springboot+mybatis-plus+fastjson

前端技术：html+css+vue+axios+element-ui+echarts

### 成果展示

用户->注册/登录
<img width="1910" height="1045" alt="用户-注册登录" src="https://github.com/user-attachments/assets/a554d6de-1eb2-4d6f-bcb4-e4930ca7c4a8" />

用户->前台首页
<img width="1910" height="1045" alt="用户-前台首页" src="https://github.com/user-attachments/assets/558b2f76-f752-4612-9eb4-d139055d11f9" />

用户->家政人员
<img width="1910" height="1045" alt="用户-家政人员" src="https://github.com/user-attachments/assets/2a6c596b-528d-4dd0-a609-0699a389a1cd" />

用户->公告信息
<img width="1910" height="1045" alt="用户-公告信息" src="https://github.com/user-attachments/assets/1a4916d5-9360-418e-b443-f0061d7091cf" />

家政人员->注册/登录
<img width="1910" height="1045" alt="家政人员-注册登录" src="https://github.com/user-attachments/assets/f9e368ac-f83f-46a0-b8e7-0ae2b5236f72" />

即时通讯管理->即时通讯
<img width="1910" height="1045" alt="即时通讯管理-即时通讯" src="https://github.com/user-attachments/assets/b7b95adb-0109-4b75-ab74-6035ca2d4587" />

通讯回复管理->通讯回复
<img width="1910" height="1045" alt="通讯回复管理-通讯回复" src="https://github.com/user-attachments/assets/d1a3f61b-a21a-41f9-ac9f-1e35708a57f1" />

预约订单管理->预约订单
<img width="1910" height="1045" alt="预约订单管理-预约订单" src="https://github.com/user-attachments/assets/a9958233-44a0-4fe6-8289-7fd462bfbf4c" />

接单信息管理->接单信息
<img width="1910" height="1045" alt="接单信息管理-接单信息" src="https://github.com/user-attachments/assets/2a0a2221-1315-4efc-ae4a-0560b3a7bbc7" />

服务费用管理->服务费用
<img width="1910" height="1045" alt="服务费用管理-服务费用" src="https://github.com/user-attachments/assets/657a8ce1-7bfb-48e6-8858-6a9303aba965" />

服务评价管理->服务评价
<img width="1910" height="1045" alt="服务评价管理-服务评价" src="https://github.com/user-attachments/assets/7a3fccf5-d871-4d8a-8ddb-b7b140576e32" />

管理员->用户管理
<img width="1910" height="1045" alt="管理员-用户管理" src="https://github.com/user-attachments/assets/0d3f1c91-75f6-4897-ad5f-37a74bea7a29" />

管理员->家政人员管理
<img width="1910" height="1045" alt="管理员-家政人员管理" src="https://github.com/user-attachments/assets/7e8b5e18-8bba-43af-948a-2be3d2551b17" />

管理员->服务类别管理
<img width="1910" height="1045" alt="管理员-服务类别管理" src="https://github.com/user-attachments/assets/cef327d8-558c-4d72-8ad0-9daff5735a95" />

系统管理->公告信息
<img width="1910" height="1045" alt="系统管理-公告信息" src="https://github.com/user-attachments/assets/ff8c7a1f-033b-4562-be03-2e38e55dfb66" />

### 源码展示

@RequestMapping("users")

@RestController

public class UsersController{

@Autowired

private UsersService userService;

@Autowired

private TokenService tokenService;

//列表

@RequestMapping("/list")

public R list( UsersEntity user){

    EntityWrapper<UsersEntity> ew = new EntityWrapper<UsersEntity>();
    ew.allEq(MPUtil.allEQMapPre( user, "user")); 
    return R.ok().put("data", userService.selectListView(ew));
    
}

//信息

@RequestMapping("/info/{id}")

public R info(@PathVariable("id") String id){

    UsersEntity user = userService.selectById(id);
    return R.ok().put("data", user);
    
}

//获取用户的session用户信息

@RequestMapping("/session")

public R getCurrUser(HttpServletRequest request){

    Long id = (Long)request.getSession().getAttribute("userId");
    UsersEntity user = userService.selectById(id);
    return R.ok().put("data", user);
    
}

//保存

@PostMapping("/save")

public R save(@RequestBody UsersEntity user){

    if(userService.selectOne(new EntityWrapper<UsersEntity>().eq("username", user.getUsername())) !=null) {
      return R.error("用户已存在");
    }
    userService.insert(user);
    return R.ok();
    
}

//修改

@RequestMapping("/update")

public R update(@RequestBody UsersEntity user){

    UsersEntity u = userService.selectOne(new EntityWrapper<UsersEntity>().eq("username", user.getUsername()));
    if(u!=null && u.getId()!=user.getId() && u.getUsername().equals(user.getUsername())) {
      return R.error("用户名已存在。");
    }
    userService.updateById(user);//全部更新
    return R.ok();
    
}

//删除

@RequestMapping("/delete")

public R delete(@RequestBody Long[] ids){

    userService.deleteBatchIds(Arrays.asList(ids));
    return R.ok();

}

}

### 账号地址及其它说明

1、地址说明

前台系统：localhost:8081

后台系统：localhost:8082

2、账号说明

用户：zhangsan/123456

家政人员：xiaoli/123456

管理员：admin/admin

3、目录结构展示

<img width="979" height="177" alt="目录结构展示" src="https://github.com/user-attachments/assets/97c7c062-b355-415e-8eee-f7525e598a7f" />

4、项目结构展示

<img width="1714" height="871" alt="项目结构展示" src="https://github.com/user-attachments/assets/a712d47a-57c7-464b-bdfe-7678197dd94f" />

5、运行步骤

1）创建数据库、导入sql脚本

2）修改application.yml中的数据库配置文件，启动服务端

3）在admin和front目录下打开cmd，执行npm install下载依赖

4）下载完毕后启动前端npm run serve，访问端口

### 获取方式(可远程调试)

访问链接(在浏览器中手动输入下图中的地址)：

<img width="1031" height="110" alt="链接" src="https://github.com/user-attachments/assets/2f1dc25f-7348-4d18-8549-9d16230f48c1" />

若资源获取失败，可添加happy35596339(vx)或1204901965(qq)进行交流
