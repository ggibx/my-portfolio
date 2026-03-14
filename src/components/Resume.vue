<template>
  <div class="resume-container">
    <div class="resume-page">
      <!-- 个人信息 -->
      <header class="resume-header">
        <div class="header-content">
          <div class="header-left">
            <h1 class="name">个人信息</h1>
            <div class="contact-info">
              <div class="info-row">
                <span class="info-item">姓名：jd/男/2003/本科/英语4级</span>
                <span class="info-item">手机号：1111111111</span>
              </div>
              <div class="info-row">
                <span class="info-item">邮箱：1sajidjiddd@qq.com</span>
              </div>
              <div class="info-row">
                <span class="info-item">求职意向：前端开发</span>
                <span class="info-item">工作地点：北京、上海、广州、深圳、武汉</span>
              </div>
            </div>
          </div>
          <div class="header-right">
            <div class="photo-container">
              <img 
                src="/photo.png" 
                alt="个人照片" 
                class="photo"
                @error="handleImageError"
              />
            </div>
          </div>
        </div>
      </header>

      <!-- 技能清单 -->
      <section class="resume-section">
        <h2 class="section-title">技能清单</h2>
        <div class="skills-content">
          <div class="skill-category">
            <h3 class="skill-title">计算机基础</h3>
            <p class="skill-desc">熟悉计算机网络、数据结构、计算机组成原理、算法、操作系统</p>
          </div>
          <div class="skill-category">
            <h3 class="skill-title">Java</h3>
            <p class="skill-desc">熟悉 Java 语言，具备 JVM 调优和性能优化经验</p>
          </div>
          <div class="skill-category">
            <h3 class="skill-title">开发工具</h3>
            <p class="skill-desc">熟练使用 Maven、Git、IDEA 和 Docker 等开发工具，熟悉 Linux 系统操作</p>
          </div>
          <div class="skill-category">
            <h3 class="skill-title">数据库</h3>
            <p class="skill-desc">熟悉 MySQL、Redis 使用及性能优化手段</p>
          </div>
          <div class="skill-category">
            <h3 class="skill-title">框架</h3>
            <p class="skill-desc">熟悉 Spring、Spring MVC、SpringBoot、MyBatis 等框架</p>
          </div>
          <div class="skill-category">
            <h3 class="skill-title">前端</h3>
            <p class="skill-desc">熟练掌握 HTML、CSS、Javascript、React、Vue 等前端技术，前后端分离架构经验丰富</p>
          </div>
        </div>
      </section>

      <!-- 实习经历 -->
      <section class="resume-section">
        <h2 class="section-title">实习经历</h2>
        <div class="experience-item">
          <div class="experience-header">
            <span class="company">广州粤嵌</span>
            <span class="period">2025年7月 ~ 2025年10月</span>
            <span class="position">前端开发工程师</span>
          </div>
          <ul class="experience-desc">
            <li>使用 Vue + Vue Router + Axios 开发前端业务，负责商品模块、订单模块、支付模块页面开发，实现业务逻辑和接口对接</li>
            <li>优化项目维护，优化代码结构，统一代码规范，重构业务逻辑，提升页面加载速度，提高可维护性</li>
            <li>配合后端对接接口，进行功能测试，保证项目稳定运行</li>
          </ul>
        </div>
      </section>

      <!-- 项目经验 -->
      <section class="resume-section">
        <h2 class="section-title">项目经验</h2>
        
        <div class="project-item">
          <div class="project-header">
            <span class="project-name">运动打卡小程序(仿Keep)</span>
            <span class="project-period">2025年12月 ~ 2026年2月</span>
            <span class="project-role">前端开发工程师</span>
          </div>
          <p class="project-desc">项目描述：使用 uni-app 框架实现了一个运动打卡微信小程序，对标 Keep 应用体验，提供运动计划、训练打卡、数据记录、社区分享等功能，覆盖用户运动全流程，帮助用户科学运动，培养运动习惯。</p>
          <p class="project-tech"><strong>技术栈：</strong>Vue + Node.js + MySQL</p>
          <ul class="project-tasks">
            <li><strong>主要工作/主要职责：</strong>使用 Vue 开发小程序前端页面，实现运动打卡、数据记录等核心功能，提升用户体验</li>
            <li>使用 Node.js 开发后端服务，开发接口处理用户运动数据，保证接口稳定响应</li>
            <li>设计 MySQL 数据库表结构，存储用户信息、运动记录等数据，实现数据高效增删改查</li>
            <li>实现前后端功能，同步用户运动数据实时同步，确保数据展示准确</li>
            <li>部署项目，使用云服务器部署前后端项目和数据库</li>
          </ul>
        </div>

        <div class="project-item">
          <div class="project-header">
            <span class="project-name">云存储小程序</span>
            <span class="project-period">2025年6月 ~ 2025年9月</span>
            <span class="project-role">前端开发工程师</span>
          </div>
          <p class="project-desc">项目描述：一个个人和团队的云存储系统，稳定可靠的存储任何文件，支持文件分类、详细权限设置，简单易用。</p>
          <p class="project-tech"><strong>技术栈：</strong>Vue + Node.js + MySQL + 微信小程序原生 API</p>
          <ul class="project-tasks">
            <li><strong>主要工作/主要职责：</strong>使用 Vue 开发小程序前端架构，实现用户登录、购物车、结算提交、商品列表页面，实现购物页面交互和状态管理，优化移动端操作体验</li>
            <li>使用 Node.js 开发后端服务，开发商品查询、订单管理、用户登录、数据统计等业务接口，实现接口高并发响应和稳定运行</li>
            <li>设计 MySQL 数据库表结构，规划用户信息、商品数据、订单记录、支付信息等数据表，编写高效 SQL 语句实现数据的增删改查，保证数据一致性</li>
            <li>实现前后端功能，对接微信小程序授权登录接口，实现登录状态实时同步，营业数据可视化展示，保证业务流程闭环，确保数据准确性</li>
          </ul>
        </div>
      </section>

      <!-- 教育经历 -->
      <section class="resume-section">
        <h2 class="section-title">教育经历</h2>
        <table class="education-table">
          <thead>
            <tr>
              <th>学校</th>
              <th>学历及专业</th>
              <th>时间</th>
              <th>GPA</th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td>贵州师范大学</td>
              <td>本科：计算机科学与技术</td>
              <td>2022.09 - 2026.06</td>
              <td>3.1/4.5 (TOP 10%)</td>
            </tr>
          </tbody>
        </table>
      </section>

      <!-- 获奖情况 -->
      <section class="resume-section">
        <h2 class="section-title">获奖情况</h2>
        <div class="awards-content">
          <p>学习优秀奖、校级奖学金、三创赛省部级二等奖、全国大学生数学竞赛省部级二等奖</p>
        </div>
      </section>
    </div>
  </div>
</template>

<script setup>
// 简历数据可以在这里定义，或者从外部导入

// 处理图片加载错误
const handleImageError = (event) => {
  // 如果图片加载失败，可以设置一个默认占位图或隐藏
  event.target.style.display = 'none';
}
</script>

<style scoped>
.resume-container {
  max-width: 210mm;
  margin: 0 auto;
  background: white;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

.resume-page {
  padding: 20mm;
  min-height: 297mm;
  background: white;
  color: #333;
  line-height: 1.6;
}

/* ������Ϣͷ�� */
.resume-header {
  border-bottom: 2px solid #333;
  padding-bottom: 15px;
  margin-bottom: 20px;
  position: relative;
}

.header-content {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  gap: 20px;
}

.header-left {
  flex: 1;
}

.header-right {
  flex-shrink: 0;
  position: absolute;
  top: 0;
  right: 0;
}

.photo-container {
  width: 100px;
  height: 125px;
  border: 2px solid #ddd;
  background: #f9f9f9;
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
  border-radius: 4px;
}

.photo {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}

.name {
  font-size: 24px;
  font-weight: bold;
  margin-bottom: 10px;
  color: #000;
}

.contact-info {
  font-size: 13px;
}

.info-row {
  margin-bottom: 5px;
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
}

.info-item {
  color: #333;
}

/* �½ڱ��� */
.resume-section {
  margin-bottom: 25px;
  page-break-inside: avoid;
}

.section-title {
  font-size: 16px;
  font-weight: bold;
  color: #000;
  border-bottom: 1px solid #666;
  padding-bottom: 5px;
  margin-bottom: 12px;
  text-transform: uppercase;
  letter-spacing: 1px;
}

/* �����嵥 */
.skills-content {
  font-size: 13px;
}

.skill-category {
  margin-bottom: 10px;
}

.skill-title {
  font-size: 14px;
  font-weight: bold;
  color: #000;
  margin-bottom: 3px;
}

.skill-desc {
  font-size: 13px;
  color: #333;
  margin-left: 15px;
  line-height: 1.8;
}

/* ������Ŀ */
.experience-item,
.project-item {
  margin-bottom: 20px;
  page-break-inside: avoid;
}

.experience-header,
.project-header {
  display: flex;
  align-items: center;
  gap: 15px;
  margin-bottom: 8px;
  flex-wrap: wrap;
}

.company,
.project-name {
  font-size: 14px;
  font-weight: bold;
  color: #000;
}

.period,
.project-period {
  font-size: 13px;
  color: #666;
}

.position,
.project-role {
  font-size: 13px;
  color: #666;
  font-style: italic;
}

.experience-desc,
.project-tasks {
  font-size: 13px;
  color: #333;
  margin-left: 20px;
  margin-top: 8px;
  line-height: 1.8;
}

.experience-desc li,
.project-tasks li {
  margin-bottom: 6px;
}

.project-desc {
  font-size: 13px;
  color: #333;
  margin-bottom: 8px;
  line-height: 1.8;
}

.project-tech {
  font-size: 13px;
  color: #333;
  margin-bottom: 8px;
}

/* 教育经历表格 */
.education-table {
  width: 100%;
  border-collapse: collapse;
  font-size: 13px;
  margin-top: 10px;
}

.education-table th,
.education-table td {
  border: 1px solid #ddd;
  padding: 10px;
  text-align: left;
}

.education-table th {
  background-color: #f5f5f5;
  font-weight: bold;
  color: #000;
}

.education-table td {
  color: #333;
}

/* 获奖情况 */
.awards-content {
  font-size: 13px;
  color: #333;
  line-height: 1.8;
}

.awards-content p {
  margin-bottom: 5px;
}

/* 打印样式 */
@media print {
  .resume-container {
    max-width: 100%;
    box-shadow: none;
    margin: 0;
  }

  .resume-page {
    padding: 15mm;
    min-height: auto;
    page-break-after: avoid;
  }

  .resume-section {
    page-break-inside: avoid;
  }

  .experience-item,
  .project-item {
    page-break-inside: avoid;
  }
}

/* 响应式设计 */
@media screen and (max-width: 768px) {
  .resume-container {
    max-width: 100%;
    margin: 0;
  }

  .resume-page {
    padding: 15px;
  }

  .experience-header,
  .project-header {
    flex-direction: column;
    align-items: flex-start;
    gap: 5px;
  }

  .header-content {
    flex-direction: column;
  }

  .header-right {
    position: static;
    align-self: center;
    margin-top: 15px;
    margin-bottom: 15px;
  }

 
  .education-table {
    font-size: 12px;
  }

  .education-table th,
  .education-table td {
    padding: 8px 5px;
  }
}
</style>