# API 接口目录  
  
本目录包含所有 API 接口和数据模型的详细文档。  
  
## 接口模块概览  
  
| 模块名称 | 接口数量 | 描述 |  
|----------|----------|------|  
| [示例接口](#示例接口) | 4 | 4个示例接口供测试用 |  
  
## 示例接口  
  
4个示例接口供测试用  
  
| 方法     | 路径                        | 名称       | 详情                                    |     |
| ------ | ------------------------- | -------- | ------------------------------------- | --- |
| `GET`  | `/api/example/users`      | 分页获取用户列表 | [查看详情](./interfaces/示例接口-分页获取用户列表.md) |     |
| `POST` | `/api/example/users`      | 创建用户     | [查看详情](示例接口-创建用户.md)                  |     |
| `POST` | `/api/example/login`      | 用户登录     | [查看详情](./interfaces/示例接口-用户登录.md)     |     |
| `GET`  | `/api/example/users/{id}` | 获取用户信息   | [查看详情](示例接口-获取用户信息.md)                |     |

  
## 数据模型  
  
| 名称                            | 说明     | 详情                                               |     |
| ----------------------------- | ------ | ------------------------------------------------ | --- |
| `ApiResponsePageResponseUser` |        | [查看详情](./schemas/ApiResponsePageResponseUser.md) |     |
| `ApiResponseString`           |        | [查看详情](./schemas/ApiResponseString.md)           |     |
| `ApiResponseUser`             |        | [查看详情](ApiResponseUser.md)                       |     |
| `LoginRequest`                |        | [查看详情](./schemas/LoginRequest.md)                |     |
| `PageResponseUser`            |        | [查看详情](./schemas/PageResponseUser.md)            |     |
| `User`                        | 用户基础信息 | [查看详情](User.md)                                  |     |
