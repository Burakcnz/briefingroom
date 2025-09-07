<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Briefing Room Display</title>
  <link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&family=Roboto+Mono:wght@700&display=swap" rel="stylesheet">
  <style>
    * { box-sizing: border-box; }
    body {
      margin: 0;
      font-family: 'Roboto', sans-serif;
      background: linear-gradient(135deg, #1a1a1a, #2b2b2b);
      color: #f0f0f0;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      overflow: hidden;
    }
    .screen { display: none; flex-direction: column; align-items: center; justify-content: center; width: 100%; height: 100%; position: relative; transition: all 0.3s ease; }
    .active { display: flex; }

    /* K1 */
    .k1-container {
      background: rgba(255,255,255,0.05);
      padding: 50px 80px;
      border-radius: 20px;
      text-align: center;
      animation: fadeIn 1s ease;
      border: 2px solid #444;
    }
    .title { font-size: 34px; font-weight: 700; margin-bottom: 30px; letter-spacing: 1px; }
    .title span { color: #ff4c4c; }
    .input-box {
      width: 260px;
      padding: 14px;
      font-size: 20px;
      border-radius: 10px;
      border: 1px solid #555;
      margin-bottom: 20px;
      outline: none;
      text-align: center;
      background: rgba(255,255,255,0.08);
      color: #f0f0f0;
    }
    .input-box::placeholder { color: #ccc; }
    .btn { padding: 14px 50px; font-size: 20px; font-weight: 700; background: #ff4c4c; border: none; border-radius: 10px; cursor: pointer; transition: all 0.3s ease; }
    .btn:hover { background: #e04343; }

    /* K2 */
    .top-left-btn {
      position: absolute;
      top: 20px;
      left: 20px;
      background: #ff4c4c;
      padding: 8px 20px;
      font-size: 16px;
      font-weight: 700;
      border-radius: 8px;
      border: none;
      color: white;
      cursor: pointer;
      opacity: 0;
      transition: opacity 0.5s ease;
    }
    .top-left-btn.show { opacity: 1; }
    .animation-panel { background: white; width: 100%; height: 250px; display: flex; justify-content: center; align-items: center; margin-bottom: 30px; box-shadow: 0 0 25px rgba(0,0,0,0.3); }
    .code-display { font-size: 120px; font-weight: 700; letter-spacing: 18px; font-family: 'Roboto Mono', monospace; color: #ff4c4c; }
    .subtitle { background: #333; color: #ff4c4c; padding: 10px 50px; font-weight: 700; margin-bottom: 15px; border-radius: 8px; letter-spacing: 2px; }
    .brief { font-size: 24px; font-style: italic; opacity: 0.9; }
    .logo { position: absolute; top: 20px; right: 20px; width: 70px; height: 70px; opacity: 0.9; }

    @keyframes fadeIn { from {opacity: 0; transform: scale(0.95);} to {opacity: 1; transform: scale(1);} }
  </style>
</head>
<body>

  <!-- K1 -->
  <div id="screen1" class="screen active">
    <div class="k1-container">
      <div class="title">BRIEFING ROOM - <span id="randomCode">XYZ</span></div>
      <input id="codeInput" class="input-box" maxlength="3" placeholder="">
      <button class="btn" id="pickBtn">Pick</button>
    </div>
  </div>

  <!-- K2 -->
  <div id="screen2" class="screen">
    <button id="newBtn" class="top-left-btn">tap for new destination</button>
    <div class="animation-panel">
      <div id="output" class="code-display">---</div>
    </div>
    <div class="subtitle">CODE</div>
    <div id="briefText" class="brief">in briefing</div>

    <!-- SVG Logo -->
    <svg class="logo" viewBox="0 0 50 50" xmlns="http://www.w3.org/2000/svg">
      <path fill="#C90019" d="M46.334 20.245c2.076 12.327-6.253 24.013-18.58 26.089-2.075.359-4.126.41-6.124.205-6.433-.666-12.225-4.1-15.966-9.226l36.673-10.507c.538-.154.384-.666-.052-.615L26.91 27.267c2.178-2.101 3.818-4.664 3.613-8.2-.41-5.741-6.253-11.353-17.119-15.044a23.5 23.5 0 0 1 6.843-2.332 22.4 22.4 0 0 1 6.125-.205c9.866 1.025 18.22 8.508 19.963 18.785zm-44.668 7.51a22.6 22.6 0 0 0 3.178 8.354c5.843-1.615 10.968-4.716 12.813-11.584 2-7.56-.282-15.479-5.766-19.656A22.68 22.68 0 0 0 1.666 27.754"></path>
    </svg>
  </div>

  <script>
    function randomLetter(){ const letters="ABCDEFGHIJKLMNOPQRSTUVWXYZ"; return letters[Math.floor(Math.random()*letters.length)]; }
    function random3Letters(){ return randomLetter()+randomLetter()+randomLetter(); }

    setInterval(()=>{ if(document.getElementById("screen1").classList.contains("active")){ document.getElementById("randomCode").textContent = random3Letters(); } },500);

    function animateLetter(finalLetter,index,callback){
      let count=0;
      const interval=setInterval(()=>{
        const current=document.getElementById("output").textContent.split("");
        if(current.length<3) current[0]=current[1]=current[2]='-';
        current[index]=randomLetter();
        document.getElementById("output").textContent=current.join("");
        count++;
        if(count>15){
          clearInterval(interval);
          const currentFinal=document.getElementById("output").textContent.split("");
          currentFinal[index]=finalLetter;
          document.getElementById("output").textContent=currentFinal.join("");
          if(callback) callback();
        }
      },80);
    }

    const pickBtn=document.getElementById("pickBtn");
    const briefText=document.getElementById("briefText");

    pickBtn.addEventListener("click",()=>{
      const input=document.getElementById("codeInput").value.toUpperCase();
      if(input.length!==3){ alert("Please enter 3 letters!"); return; }
      document.getElementById("screen1").classList.remove("active");
      document.getElementById("screen2").classList.add("active");
      document.getElementById("output").textContent="---";
      document.getElementById("newBtn").classList.remove("show");
      briefText.textContent="in briefing";
      let i=0;
      function next(){ if(i<3){ animateLetter(input[i],i,next); i++; } else { document.getElementById("newBtn").classList.add("show"); } }
      next();
    });

    document.getElementById("newBtn").addEventListener("click",()=>{
      document.getElementById("screen2").classList.remove("active");
      document.getElementById("screen1").classList.add("active");
      document.getElementById("codeInput").value="";
      document.getElementById("newBtn").classList.remove("show");
      document.getElementById("output").textContent="---";
      briefText.textContent="in briefing";
    });
  </script>

</body>
</html>
