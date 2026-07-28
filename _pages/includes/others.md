# 👨‍🏫 Teaching
- *2026 Spring*, CS2042 Comprehensive Experiment of Programming.
- *2026 Spring*, DS2031 Application of Statistical Software.
- *2025 Fall*, CS1021 Introduction to Computer Science.
- *2025 Fall*, AI1002 Introduction to Artificial Intelligence.


# 📖 Educations
- *2022.09 - 2025.08*, Ph.D. in Management, [The Hong Kong University of Science and Technology](https://hkust.edu.hk/), Hong Kong.
- *2017.09 - 2021.06*, B.Sc. in Engineering, [South China University of Technology](https://www.scut.edu.cn/en/), Guangzhou.


# 💻 Internships
- *2021.01 - 2021.06*, [Tencent](https://www.tencent.com/), Algorithm Researcher, Shenzhen.


# 💾 Resources
- 📂 [Google Scholar](https://scholar.google.com), [Overleaf](https://www.overleaf.com), [Doubao](https://www.doubao.com)
- 📖 [JCRAD](https://crad.ict.ac.cn/), [EAAI](https://www.sciencedirect.com/journal/engineering-applications-of-artificial-intelligence/articles-in-press), [TFSC](https://www.sciencedirect.com/journal/technological-forecasting-and-social-change/articles-in-press)
- 📺 [Bilibili](https://www.bilibili.com), [YouTube](https://www.youtube.com), [TVB(翡翠台)](https://www.mytvsuper.com/tc/live/81/%E7%BF%A1%E7%BF%A0%E5%8F%B0/?gad_source=1&gad_campaignid=22327305104&gbraid=0AAAAADApjTV_qoSamALWOWo7FKO6T0VY0&gclid=CjwKCAjwyOzSBhBTEiwAmxvJ-ujxbd-Fo23MCOHPhck57kxMzmGnSi3T8UYHLhias-rl-dzisaCazRoCGmMQAvD_BwE), [FengShows](https://www.fengshows.com/live)

<!-- Font Awesome -->
<link rel="stylesheet" href="https://cdn.bootcdn.net/ajax/libs/font-awesome/6.4.0/css/all.min.css">
<div id="music-player-bar">
  <audio id="main-audio" preload="auto"></audio>
<div class="player-row">
    <button id="prev-btn"><i class="fa-solid fa-backward-step"></i></button>

    <button id="play-pause-btn">
      <i class="fa-solid fa-play"></i>
    </button>

    <button id="next-btn"><i class="fa-solid fa-forward-step"></i></button>

    <button id="mode-btn" class="mode-btn">
      <i class="fa-solid fa-repeat"></i>
    </button>

    <span id="track-name" class="track-name">Loading...</span>

    <span id="current-time" class="time">0:00</span>

    <input type="range" id="progress-bar" min="0" max="100" value="0">

    <span id="duration" class="time">0:00</span>

    <i id="volume-icon" class="fa-solid fa-volume-low volume-icon"></i>

    <input type="range" id="volume-slider" min="0" max="100" value="20">

    <span id="volume-display" class="volume-text">20%</span>
</div>
</div>
<script>
// =========【你只需要修改这里！！】==========
// 填写static/music下所有mp3，路径以 /music/ 开头，自定义歌曲名称
const songList = [
    {
        name: "千個晨早",
        src: "/music/周慧敏(千個晨早).mp3"
    },
    {
        name: "也許應該分手了",
        src: "/music/周慧敏(也許應該分手了).mp3"
    }
];
// ========================================

let currentIndex = 0;
let loopMode = 0; // 0:列表循环  1:单曲循环  2:随机播放

// DOM元素
const audio = document.getElementById('main-audio');
const playBtn = document.getElementById('play-pause-btn');
const playIcon = playBtn.querySelector('i');
const prevBtn = document.getElementById('prev-btn');
const nextBtn = document.getElementById('next-btn');
const modeBtn = document.getElementById('mode-btn');
const trackName = document.getElementById('track-name');
const progressBar = document.getElementById('progress-bar');
const currentTimeEl = document.getElementById('current-time');
const durationEl = document.getElementById('duration');
const volumeSlider = document.getElementById('volume-slider');
const volumeDisplay = document.getElementById('volume-display');

// 初始化音量
audio.volume = 0.2;

// 加载当前歌曲
function loadSong(index) {
    const song = songList[index];
    audio.src = song.src;
    trackName.textContent = song.name;
}

// 时间格式化 秒 → 00:00
function formatTime(sec) {
    const m = Math.floor(sec / 60);
    const s = Math.floor(sec % 60);
    return `${m}:${s.toString().padStart(2, '0')}`;
}

// 播放/暂停切换
function togglePlay() {
    if(audio.paused){
        audio.play();
        playIcon.className = "fa-solid fa-pause";
    }else{
        audio.pause();
        playIcon.className = "fa-solid fa-play";
    }
}

// 下一首
function nextSong() {
    if(loopMode === 2){
        //随机
        currentIndex = Math.floor(Math.random() * songList.length);
    }else{
        currentIndex = (currentIndex + 1) % songList.length;
    }
    loadSong(currentIndex);
    audio.play();
    playIcon.className = "fa-solid fa-pause";
}

// 上一首
function prevSong() {
    if(loopMode === 2){
        currentIndex = Math.floor(Math.random() * songList.length);
    }else{
        currentIndex = (currentIndex - 1 + songList.length) % songList.length;
    }
    loadSong(currentIndex);
    audio.play();
    playIcon.className = "fa-solid fa-pause";
}

// 切换循环模式
function changeMode() {
    loopMode = (loopMode + 1) % 3;
    const icon = modeBtn.querySelector('i');
    if(loopMode === 0){
        icon.className = "fa-solid fa-repeat";
        modeBtn.title="列表循环";
    }else if(loopMode ===1){
        icon.className = "fa-solid fa-repeat-1";
        modeBtn.title="单曲循环";
    }else{
        icon.className = "fa-solid fa-shuffle";
        modeBtn.title="随机播放";
    }
}

// 歌曲播放结束自动切歌
audio.addEventListener('ended', ()=>{
    if(loopMode === 1){
        //单曲循环
        audio.currentTime=0;
        audio.play();
    }else{
        nextSong();
    }
});

// 进度条同步
audio.addEventListener('timeupdate',()=>{
    const percent = (audio.currentTime / audio.duration) *100 || 0;
    progressBar.value = percent;
    currentTimeEl.textContent = formatTime(audio.currentTime);
});

// 歌曲加载完成显示总时长
audio.addEventListener('loadedmetadata',()=>{
    durationEl.textContent = formatTime(audio.duration);
});

// 拖动进度条跳转
progressBar.addEventListener('input',()=>{
    const seekTime = (progressBar.value /100) * audio.duration;
    audio.currentTime = seekTime;
});

// 音量调节
volumeSlider.addEventListener('input',()=>{
    const vol = volumeSlider.value /100;
    audio.volume = vol;
    volumeDisplay.textContent = volumeSlider.value + "%";
});

// 按钮绑定事件
playBtn.addEventListener('click', togglePlay);
nextBtn.addEventListener('click', nextSong);
prevBtn.addEventListener('click', prevSong);
modeBtn.addEventListener('click', changeMode);

// 页面初始化加载第一首
loadSong(currentIndex);
</script>

