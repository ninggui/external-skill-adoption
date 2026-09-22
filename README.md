<div align="center">

# external-skill-adoption

**给一个外部 GitHub 仓库，按"定位→审查→安装→A/B实测→沉淀"五步决定要不要用。**

<p>
  <a href="#"><img src="https://img.shields.io/badge/flow-5%20steps-blue" alt="5 steps" /></a>
</p>

[流程](#评估流程)

</div>

---

## 评估流程

1. **定位本体**：GitHub API 搜仓库、读 README，区分整合包和本体
2. **判断依赖**：绑特定运行时？模型无关？能否直接装？
3. **审查代码**：读核心文件，看有没有硬编码密钥/过度承诺
4. **A/B 实测**：装了跑一遍，对比不装时的效果差异
5. **沉淀结论**：有用就保留+写使用说明，没用就记黑名单

## License

MIT
