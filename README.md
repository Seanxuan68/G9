<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>未来专业推荐系统</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      padding: 2rem;
      background: #f2f2f2;
    }
    .keyword {
      display: inline-block;
      margin: 0.5rem;
      padding: 0.5rem 1rem;
      background: #fff;
      border: 2px solid #007bff;
      border-radius: 1rem;
      cursor: pointer;
    }
    .keyword.selected {
      background: #007bff;
      color: white;
    }
    .result {
      margin-top: 2rem;
      padding: 1rem;
      background: white;
      border-radius: 1rem;
      box-shadow: 0 0 5px rgba(0,0,0,0.1);
    }
  </style>
</head>
<body>
  <h1>选择你的关键词：</h1>
  <div id="keywords"></div>
  <button onclick="generateResult()">生成推荐</button>

  <div class="result" id="result"></div>

  <script>
    const keywordList = [
      "🎮游戏叙事", "⚖️算法伦理", "👥团队赋能",
      "🎨数字艺术", "📈智能金融", "🌍公益创新",
      "🧠心理科技", "💡社会企业", "📊大数据",
      "🤖机器人", "🔭深空探索", "🏗️基建工程",
      "📉经济", "📐数学", "🧪化学",
      "⚙️工程", "💻电子电气", "🏥医学",
      "⚖️法律", "📚文学", "📜历史",
      "🌐语言", "🗺️地理", "🧭政治",
      "🎥传媒", "🛫航空", "🧑‍🏫教育",
      "📊管理", "📘会计与金融", "🧬生物"
    ];

    const RIASECMap = {
      "🎮游戏叙事": "Artistic",
      "⚖️算法伦理": "Investigative",
      "👥团队赋能": "Social",
      "🎨数字艺术": "Artistic",
      "📈智能金融": "Conventional",
      "🌍公益创新": "Social",
      "🧠心理科技": "Investigative",
      "💡社会企业": "Enterprising",
      "📊大数据": "Investigative",
      "🤖机器人": "Realistic",
      "🔭深空探索": "Investigative",
      "🏗️基建工程": "Realistic",
      "📉经济": "Conventional",
      "📐数学": "Investigative",
      "🧪化学": "Investigative",
      "⚙️工程": "Realistic",
      "💻电子电气": "Realistic",
      "🏥医学": "Social",
      "⚖️法律": "Enterprising",
      "📚文学": "Artistic",
      "📜历史": "Artistic",
      "🌐语言": "Artistic",
      "🗺️地理": "Investigative",
      "🧭政治": "Enterprising",
      "🎥传媒": "Artistic",
      "🛫航空": "Realistic",
      "🧑‍🏫教育": "Social",
      "📊管理": "Enterprising",
      "📘会计与金融": "Conventional",
      "🧬生物": "Investigative"
    };

    const subjectExamples = {
      "🎮游戏叙事": ["游戏设计", "互动媒体"],
      "⚖️算法伦理": ["人工智能伦理", "科技哲学"],
      "👥团队赋能": ["组织行为学", "人力资源管理"],
      "🎨数字艺术": ["数字媒体", "创意产业"],
      "📈智能金融": ["金融科技", "计量金融"],
      "🌍公益创新": ["发展学", "社会创新"],
      "🧠心理科技": ["心理学与技术", "行为科学"],
      "💡社会企业": ["社会企业管理", "可持续商业"],
      "📊大数据": ["数据科学", "商业分析"],
      "🤖机器人": ["机器人工程", "智能系统"],
      "🔭深空探索": ["航天工程", "天体物理"],
      "🏗️基建工程": ["土木工程", "可持续建设"],
      "📉经济": ["经济学", "行为经济学"],
      "📐数学": ["数学", "应用数学"],
      "🧪化学": ["化学", "药物化学"],
      "⚙️工程": ["机械工程", "材料工程"],
      "💻电子电气": ["电子工程", "电气与计算机工程"],
      "🏥医学": ["医学", "生物医学科学"],
      "⚖️法律": ["法学", "国际法"],
      "📚文学": ["英语文学", "比较文学"],
      "📜历史": ["历史", "现代史"],
      "🌐语言": ["语言学", "翻译与交流"],
      "🗺️地理": ["人文地理", "环境地理"],
      "🧭政治": ["政治学", "国际关系"],
      "🎥传媒": ["媒体与传播", "数字传播"],
      "🛫航空": ["航空工程", "航空管理"],
      "🧑‍🏫教育": ["教育学", "教育心理学"],
      "📊管理": ["工商管理", "战略管理"],
      "📘会计与金融": ["会计", "金融"],
      "🧬生物": ["生物科学", "生物信息学"]
    };

    const selected = new Set();
    const keywordsDiv = document.getElementById("keywords");

    keywordList.forEach(word => {
      const btn = document.createElement("div");
      btn.className = "keyword";
      btn.innerText = word;
      btn.onclick = () => {
        if (selected.has(word)) {
          selected.delete(word);
          btn.classList.remove("selected");
        } else {
          if (selected.size === 3) return alert("最多选择3个关键词！");
          selected.add(word);
          btn.classList.add("selected");
        }
      };
      keywordsDiv.appendChild(btn);
    });

    function generateResult() {
      if (selected.size !== 3) {
        document.getElementById("result").innerHTML = "请先选择3个关键词！";
        return;
      }

      const selectedArr = Array.from(selected);
      const combined = selectedArr.join(', ');

      const allRIASEC = selectedArr.map(k => RIASECMap[k]);
      const RIASECCount = {};
      allRIASEC.forEach(code => {
        RIASECCount[code] = (RIASECCount[code] || 0) + 1;
      });

      const sortedRIASEC = Object.entries(RIASECCount).sort((a, b) => b[1] - a[1]);
      const topRIASEC = sortedRIASEC.map(x => x[0]).join(' / ');

      let allSubjects = [];
      selectedArr.forEach(k => {
        if (subjectExamples[k]) allSubjects = allSubjects.concat(subjectExamples[k]);
      });
      const uniq = arr => [...new Set(arr)];
      allSubjects = uniq(allSubjects);

      const slogan = `您选择的关键词组合：${combined}，表现出您偏向的RIASEC类型为：${topRIASEC}`;

      const resultHTML = `
        <h2>${slogan}</h2>
        <p><strong>推荐专业方向：</strong></p>
        <ul>${allSubjects.map(s => `<li>${s}</li>`).join('')}</ul>
        <p><strong>推荐英国大学：</strong> UCL, KCL, Edinburgh, Manchester, Warwick 等（根据专业方向匹配）</p>
      `;

      document.getElementById("result").innerHTML = resultHTML;
    }
  </script>
</body>
</html>
