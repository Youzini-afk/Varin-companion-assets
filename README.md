# Varin 桌宠素材

[Varin](https://github.com/Youzini-afk/Varin) 桌面桌宠的原画、制作源、选定下载包和动画实验。2026-10-30 从 Varin 主仓库迁出，保留了这些目录的全部提交历史。

完整历史约 1.5 GB。只需要当前文件时用 `git clone --depth 1`。

## 目录

| 目录 | 内容 | 迁出前在 Varin 的位置 |
|---|---|---|
| [`source/`](source/README.md) | 角色母版、分镜、动画源图、制作清单与审阅资料 | `docs/design/desktop-companion-assets/` |
| `pack/varin/` | 选定的运行包：`bundle.json`、`occlusion.json`、21 张彩色图集、21 张 alpha 轮廓页、世界凳图和 `acceptance.json` | `packages/web/public/companion-assets/` |
| [`motion-lab/`](motion-lab/README.md) | 连续动画独立实验（debug rig 与真实美术接线试样），不接入产品 | `tools/desktop-companion-motion-lab/` |

## Varin 如何使用

Varin 不再包含图集和制作源。它在 `packages/web/companion-assets.lock.json` 中锁定本仓库的一个提交，以及 `pack/varin/` 里每个下载文件的字节数和 SHA-256。`bundle.json` 与 `occlusion.json` 仍随播放器代码留在 Varin，构建时必须与锁定的哈希一致。

- 桌面安装包构建时由锁文件生成 `companion-catalog.json`。用户在设置里下载桌宠时，应用从 `https://raw.githubusercontent.com/Youzini-afk/Varin-companion-assets/<commit>/pack/varin/` 取文件并逐个校验。
- 已发布的 Varin 版本仍从 Varin 主仓库各自固定的提交下载。主仓库历史没有改写，这些地址继续有效。
- 开发时在 Varin 根目录运行 `bun run companion:fetch`，把锁定的图集写进 `packages/web/public/companion-assets/`。设置 `VARIN_COMPANION_ASSETS=<本仓库检出路径>` 时从本地仓库读取，不走网络。
- Varin 的 `tools/desktop-companion-animation/` 读取 `source/`，默认查找与 Varin 并排的 `../Varin-companion-assets`，也可用 `VARIN_COMPANION_ASSETS` 指定。

## 更新选定包

1. 在 Varin 中用 `tools/desktop-companion-animation/` 生成新包（输出到 `packages/web/public/companion-assets/`），并用 `build_occlusion_pages.py` 重建轮廓页。
2. 把整包复制到本仓库的 `pack/varin/`，删除不再引用的旧图，提交并推送。不要只替换部分图集。
3. 在 Varin 根目录运行 `bun run companion:lock ../Varin-companion-assets`，提交更新后的锁文件和两个描述文件。锁定前该提交必须已经推送。

## 迁出后的路径

- Markdown 中指向 Varin 设计文档的链接已改成 GitHub 地址；`source/` 与 `motion-lab/` 之间的相对路径已按新目录改写。
- `source/` 下 provenance、manifest 等 JSON 记录的 `docs/design/desktop-companion-assets/...` 路径是当时的来源记录，按原样保留。
- `source/` 中部分历史生成脚本（如 `director-role-concepts/`、`project-lead-library/`、`showcase/upright-v2/` 下的 `.mjs`）读取 Varin 产品源码和依赖，默认用相对路径寻找 Varin 根目录。迁出后这个默认值不再成立：`render-showcase.mjs` 接受 `--repo`，`design.mjs` 等接受 Varin 路径作为第一个参数，其余脚本需放回 Varin 检出中原来的位置运行。

## 许可

- 角色、插画素材及角色设定的授权条款尚未确定，见 [`source/README.md`](source/README.md) 的「完整性与许可边界」。不要仅凭代码许可证推定这些图片和设定获得 AGPL 授权。
- `motion-lab/` 与 `source/` 中的脚本源码和在 Varin 中时一样，以 AGPL-3.0-only 发布，许可证全文见 [`motion-lab/LICENSE`](motion-lab/LICENSE)。
