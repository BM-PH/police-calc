<!doctype html>
<html lang="ko">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Premium Purple Calculator</title>
  <style>
    :root {
      --bg: #050508;
      --card-bg: rgba(18, 18, 28, 0.95);
      --text: #f0f2f9;
      --muted: #94a3b8;
      --accent: #a855f7;
      --accent-glow: rgba(168, 85, 247, 0.3);
      --border: rgba(168, 85, 247, 0.25);
      --radius: 20px;
      --danger: #f43f5e;
    }

    * { box-sizing: border-box; -webkit-font-smoothing: antialiased; }
    body {
      margin: 0; font-family: "Pretendard Variable", sans-serif;
      background: radial-gradient(circle at 0% 0%, rgba(168, 85, 247, 0.1) 0%, transparent 40%), var(--bg);
      color: var(--text); min-height: 100vh; line-height: 1.6; overflow-x: hidden;
    }

    .wrap { max-width: 1200px; margin: 0 auto; padding: 40px 20px; }
    header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 32px; }
    h1 { margin: 0; font-size: 24px; font-weight: 850; color: #fff; text-shadow: 0 0 15px var(--accent-glow); }

    .grid { display: grid; grid-template-columns: 1fr 380px; gap: 24px; }
    @media (max-width: 1024px) { .grid { grid-template-columns: 1fr; } }

    .card { background: var(--card-bg); border: 1px solid var(--border); border-radius: var(--radius); overflow: hidden; backdrop-filter: blur(15px); }
    .card-hd { padding: 20px; border-bottom: 1px solid var(--border); background: rgba(168, 85, 247, 0.08); }

    .tabs { display: flex; gap: 8px; overflow-x: auto; padding-bottom: 4px; }
    .tab {
      padding: 10px 18px; border-radius: 12px; border: 1px solid var(--border);
      background: rgba(255,255,255,0.02); color: var(--muted); cursor: pointer; font-size: 13px; font-weight: 600; transition: 0.2s; white-space: nowrap;
    }
    .tab.active { background: var(--accent); color: white; border-color: var(--accent); box-shadow: 0 0 15px var(--accent-glow); }

    .item-list { padding: 20px; display: flex; flex-direction: column; gap: 12px; }
    
    .item {
      display: flex; justify-content: space-between; align-items: center;
      padding: 18px; background: rgba(255,255,255,0.03); border: 1px solid var(--border); border-radius: 16px; transition: 0.2s; cursor: pointer;
    }
    .item:hover { border-color: var(--accent); background: rgba(168, 85, 247, 0.1); transform: translateX(5px); }
    .item.selected { border: 1px solid var(--accent); background: rgba(168, 85, 247, 0.15); box-shadow: 0 0 15px rgba(168, 85, 247, 0.1); }

    .item-info .name { font-weight: 700; font-size: 16px; color: #fff; }
    .item-info .details { font-size: 12px; color: var(--muted); margin-top: 4px; }

    .check-mark { color: var(--accent); font-size: 20px; font-weight: 900; opacity: 0; transition: 0.2s; }
    .item.selected .check-mark { opacity: 1; }

    .metric-card { padding: 24px; border-radius: 18px; background: rgba(0,0,0,0.3); border: 1px solid var(--border); margin-bottom: 20px; }
    .metric-card.highlight { border-color: var(--accent); background: rgba(168, 85, 247, 0.05); }
    .metric-card.warning { border-color: var(--danger); background: rgba(244, 63, 94, 0.05); }
    .metric-card .label { font-size: 12px; color: var(--accent); font-weight: 800; margin-bottom: 8px; }
    .metric-card.warning .label { color: var(--danger); }
    .metric-card .value { font-size: 28px; font-weight: 900; }
    
    .detention-warning { color: var(--danger); font-size: 13px; font-weight: 800; margin-top: 10px; display: none; animation: blink 1s infinite; }
    @keyframes blink { 50% { opacity: 0.5; } }

    .ko-read { font-size: 12px; color: var(--muted); margin-top: 8px; border-top: 1px solid rgba(255,255,255,0.05); padding-top: 8px; }

    .btn { padding: 12px 24px; border-radius: 12px; border: none; font-weight: 800; cursor: pointer; transition: 0.2s; }
    .btn-primary { background: var(--accent); color: white; }
    .btn-ghost { background: transparent; color: var(--muted); border: 1px solid var(--border); }

    #copyToast {
      position: fixed; bottom: 30px; left: 50%; transform: translateX(-50%) translateY(100px);
      background: rgba(20, 20, 30, 0.95); border: 1px solid var(--accent); border-radius: 16px;
      padding: 16px 24px; min-width: 280px; transition: 0.4s; visibility: hidden; z-index: 11000;
    }
    #copyToast.show { transform: translateX(-50%) translateY(0); visibility: visible; }
    .progress-bar { width: 0%; height: 4px; background: var(--accent); border-radius: 2px; margin-top: 8px; }
  </style>
</head>
<body>

  <div class="wrap">
    <header>
      <h1>엑시트 경찰청 법률계산기</h1>
      <div style="display: flex; gap: 10px;">
        <button class="btn btn-ghost" onclick="location.reload()">초기화</button>
        <button class="btn btn-primary" id="btnCopy">법률리스트 복사</button>
      </div>
    </header>

    <div class="grid">
      <section class="card">
        <div class="card-hd"><div class="tabs" id="tabs"></div></div>
        <div class="item-list" id="crimeList"></div>
      </section>

      <aside>
        <div class="card">
          <div class="card-hd"><span style="font-weight: 800; color: var(--accent);">계산 결과</span></div>
          <div style="padding: 20px;">
            <div class="metric-card highlight">
              <div class="label">최종 합계 벌금</div>
              <div class="value" id="totalFine">0원</div>
              <div class="ko-read" id="totalFineKo">영 원</div>
            </div>
            
            <div class="metric-card" id="timeCard">
              <div class="label">최종 구금 시간</div>
              <div class="value" id="totalTime">0분</div>
              <div class="detention-warning" id="timeWarning">⚠️ 최대 구금 시간(120분) 초과!</div>
            </div>

            <div class="metric-card">
              <div class="label">보석금 (분당 500만)</div>
              <input type="number" id="bailMinutes" value="0" min="0" style="width: 100%; background: #000; border: 1px solid var(--border); color: white; padding: 10px; border-radius: 10px;" />
              <div class="ko-read" id="bailSub">보석금: 0원</div>
            </div>
            <div id="summaryRows" style="font-size: 12px; color: var(--muted); border-top: 1px solid var(--border); padding-top: 15px;"></div>
          </div>
        </div>
      </aside>
    </div>
  </div>

  <div id="copyToast">
    <div style="text-align:center; font-weight:700;">📋 복사되었습니다!</div>
    <div class="progress-bar" id="toastBar"></div>
  </div>

<script>
  const DATA = {
    "도로교통법": [
      { name:"소음공해", fine: 10000000, time: 0 }, { name:"불법 주정차", fine: 10000000, time: 0 },
      { name:"속도 위반", fine: 10000000, time: 0 }, { name:"신호 위반", fine: 10000000, time: 0 },
      { name:"불법 유턴", fine: 10000000, time: 0 }, { name:"역주행 / 차선 위반", fine: 10000000, time: 0 },
      { name:"인도 주행", fine: 10000000, time: 0 }, { name:"공공기물 파손", fine: 15000000, time: 0 },
      { name:"스턴트", fine: 20000000, time: 0 }, { name:"차량파손", fine: 50000000, time: 10 },
      { name:"뺑소니", fine: 100000000, time: 20 }, { name:"폭주(350km/h초과)", fine: 50000000, time: 20 },
      { name:"항공기 저공 비행", fine: 300000000, time: 40 }, { name:"보복운전", fine: 100000000, time: 20 },
      { name:"무허가 항공기 운행", fine: 150000000, time: 30 }
    ],
    "중범죄": [
      { name:"난폭운전(도로교통법3개위반)", fine: 50000000, time: 20 }, { name:"명예훼손", fine: 20000000, time: 10 },
      { name:"폭행", fine: 70000000, time: 10 }, { name:"불법 물건 소지", fine: 80000000, time: 20 },
      { name:"불법 총기 소지", fine: 150000000, time: 20 }, { name:"불법 무기/물건 언급", fine: 100000000, time: 20 },
      { name:"증거 인멸", fine: 120000000, time: 20 }, { name:"차량 절도", fine: 100000000, time: 20 },
      { name:"시민 살인", fine: 200000000, time: 30 }
    ],
    "공무원법": [
      { name:"공무원 차량 절도 시도", fine: 100000000, time: 0 }, { name:"공무원 차량 절도", fine: 250000000, time: 20 },
      { name:"공무원 명예훼손", fine: 200000000, time: 10 }, { name:"공무원 차량 파손", fine: 100000000, time: 10 },
      { name:"공무원 사칭", fine: 50000000, time: 10 }, { name:"공무집행 방해", fine: 150000000, time: 30 },
      { name:"국유지/사유지 침입", fine: 200000000, time: 30 }, { name:"공무원 폭행", fine: 250000000, time: 20 },
      { name:"공무원 살인", fine: 400000000, time: 40 }
    ],
    "스토리RP": [
      { name:"즉흥RP", fine: 200000000, time: 25 },
      { name:"수배RP", fine: 300000000, time: 30 },
      { name:"영장RP", fine: 400000000, time: 30 },
      { name:"납치RP", fine: 200000000, time: 10 },
      { name:"빈집털이RP", fine: 30000000, time: 10 },
      { name:"ATM RP", fine: 50000000, time: 10 },
      { name:"편의점 RP", fine: 70000000, time: 10 },
      { name:"보석상 RP", fine: 100000000, time: 20 },
      { name:"은행 RP", fine: 150000000, time: 30 },
      { name:"닭공장 RP", fine: 150000000, time: 30 },
      { name:"클럽 RP", fine: 100000000, time: 20 },
      { name:"도주 RP", fine: 100000000, time: 20 }
    ]
  };

  let activeCategory = Object.keys(DATA)[0];
  const state = new Map();

  function convertToKo(num) {
    if (num === 0) return "영 원";
    const units = ["", "만", "억", "조"];
    const nums = ["", "일", "이", "삼", "사", "오", "육", "칠", "팔", "구"];
    let result = []; let unitIdx = 0; let tempNum = num;
    while (tempNum > 0) {
      let chunk = tempNum % 10000;
      if (chunk > 0) {
        let chunkStr = ""; let digits = chunk.toString().split("").reverse();
        digits.forEach((d, i) => { if (d !== "0") chunkStr = nums[Number(d)] + ["", "십", "백", "천"][i] + chunkStr; });
        result.push(chunkStr + units[unitIdx]);
      }
      tempNum = Math.floor(tempNum / 10000); unitIdx++;
    }
    return result.reverse().join(" ") + " 원";
  }

  function update(){
    let fRaw = 0, tRaw = 0; let rows = "";
    state.forEach((st) => {
      if(st.checked){
        fRaw += st.item.fine; tRaw += st.item.time;
        rows += `<div style="display:flex; justify-content:space-between; margin-bottom:4px;"><span>${st.item.name}</span><span>${st.item.fine.toLocaleString()}원</span></div>`;
      }
    });

    const bMin = parseInt(document.getElementById("bailMinutes").value)||0;
    const reduction = Math.min(bMin, tRaw);
    const finalTotalFine = fRaw + (reduction * 5000000);
    const finalTime = tRaw - reduction;

    document.getElementById("totalFine").textContent = finalTotalFine.toLocaleString() + "원";
    document.getElementById("totalFineKo").textContent = convertToKo(finalTotalFine);
    document.getElementById("totalTime").textContent = finalTime + "분";
    document.getElementById("bailSub").textContent = `보석금: +${(reduction*5000000).toLocaleString()}원`;
    document.getElementById("summaryRows").innerHTML = rows;

    // 120분 초과 경고 로직
    const timeCard = document.getElementById("timeCard");
    const timeWarning = document.getElementById("timeWarning");
    if (finalTime > 120) {
      timeCard.classList.add("warning");
      timeWarning.style.display = "block";
    } else {
      timeCard.classList.remove("warning");
      timeWarning.style.display = "none";
    }
  }

  function toggleItem(cat, name) {
    const key = `${cat}::${name}`;
    const st = state.get(key);
    st.checked = !st.checked;
    renderList();
    update();
  }

  function renderList(){
    const $list = document.getElementById("crimeList"); $list.innerHTML = "";
    DATA[activeCategory].forEach(item => {
      const key = `${activeCategory}::${item.name}`;
      if(!state.has(key)) state.set(key, { checked:false, item });
      const st = state.get(key);
      const el = document.createElement("div");
      el.className = `item ${st.checked?'selected':''}`;
      el.innerHTML = `
        <div class="item-info">
          <div class="name">${item.name}</div>
          <div class="details">${item.fine.toLocaleString()}원 / ${item.time}분</div>
        </div>
        <div class="check-mark">✓</div>
      `;
      el.onclick = () => toggleItem(activeCategory, item.name);
      $list.appendChild(el);
    });
  }

  function renderTabs(){
    const $tabs = document.getElementById("tabs"); $tabs.innerHTML = "";
    Object.keys(DATA).forEach(cat => {
      const b = document.createElement("button"); b.className = "tab "+(cat===activeCategory?'active':'');
      b.textContent = cat; b.onclick = () => { activeCategory = cat; renderTabs(); renderList(); };
      $tabs.appendChild(b);
    });
  }

  document.getElementById("bailMinutes").oninput = update;

  document.getElementById("btnCopy").onclick = async () => {
    let t = "[벌금 산정 내역]\n\n";
    state.forEach(st => { if(st.checked){ t += `- ${st.item.name} (${st.item.fine.toLocaleString()}원 / ${st.item.time}분)\n`; }});
    t += `\n최종 벌금: ${document.getElementById("totalFine").textContent}\n구금 시간: ${document.getElementById("totalTime").textContent}`;
    await navigator.clipboard.writeText(t);
    const toast = document.getElementById("copyToast"); const bar = document.getElementById("toastBar");
    toast.classList.add("show"); bar.style.width = "0%";
    let p = 0; const intv = setInterval(() => { p += 2; bar.style.width = p+"%"; if(p>=100){ clearInterval(intv); setTimeout(()=>toast.classList.remove("show"), 200); } }, 30);
  };

  renderTabs(); renderList(); update();
</script>
</body>
</html>
