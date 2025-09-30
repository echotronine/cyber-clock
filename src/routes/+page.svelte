<script lang="ts">
  import { onMount, onDestroy } from 'svelte';

  let time: Date = new Date();
  let glitchActive: boolean = false;
  let timer: number | undefined;

  onMount(() => {
    timer = setInterval(() => {
      time = new Date();

      // 随机触发故障效果
      if (Math.random() > 0.95) {
        glitchActive = true;
        setTimeout(() => glitchActive = false, 200);
      }
    }, 1000) as unknown as number;

    return () => {
      if (timer) clearInterval(timer);
    };
  });

  onDestroy(() => {
    if (timer) clearInterval(timer);
  });

  $: hours = String(time.getHours()).padStart(2, '0');
  $: minutes = String(time.getMinutes()).padStart(2, '0');
  $: seconds = String(time.getSeconds()).padStart(2, '0');

  $: dateStr = time.toLocaleDateString('zh-CN', { 
    year: 'numeric', 
    month: '2-digit', 
    day: '2-digit'
  }).replace(/\//g, '.');

  $: dayStr = ['SUN', 'MON', 'TUE', 'WED', 'THU', 'FRI', 'SAT'][time.getDay()];

  $: hourProgress = (time.getHours() / 24) * 100;
  $: minuteProgress = (time.getMinutes() / 60) * 100;
  $: secondProgress = (time.getSeconds() / 60) * 100;
</script>

<svelte:head>
  <title>Cyber Clock</title>
</svelte:head>

<div class="relative min-h-screen bg-black overflow-hidden flex items-center justify-center font-mono">
  <!-- 网格背景 -->
  <div class="absolute inset-0 grid-bg"></div>

  <!-- 扫描线效果 -->
  <div class="absolute inset-0 pointer-events-none">
    <div class="scanline w-full h-1 opacity-20"></div>
  </div>

  <!-- 噪点效果 -->
  <div class="absolute inset-0 opacity-5 pointer-events-none noise-bg"></div>

  <!-- 主要内容 -->
  <div class="relative z-10 text-center px-8">
    <!-- 顶部装饰 -->
    <div class="flex items-center justify-center gap-4 mb-8">
      <div class="h-px w-20 bg-gradient-to-r from-transparent to-cyan-400"></div>
      <div class="text-cyan-400 text-sm tracking-[0.2em]">SYSTEM TIME</div>
      <div class="h-px w-20 bg-gradient-to-l from-transparent to-cyan-400"></div>
    </div>

    <!-- 主时钟 -->
    <div class="relative" class:glitch={glitchActive}>
      <!-- 主时间显示 -->
      <div class="text-9xl font-bold text-cyan-400 mb-4 flex items-center justify-center gap-4 tracking-wider cyber-glow">
        {hours}
        <span class="inline-block text-center animate-pulse">:</span>
        {minutes}
        <span class="inline-block text-center animate-pulse">:</span>
        {seconds}
      </div>
    </div>

    <!-- 日期信息 -->
    <div class="flex items-center justify-center gap-8 mb-8 flex-wrap">
      <div class="border border-cyan-400/50 px-6 py-3 bg-cyan-400/5 cyber-border-cyan">
        <div class="text-xs text-cyan-400/70 mb-1">DATE</div>
        <div class="text-xl text-cyan-400">{dateStr}</div>
      </div>
      
      <div class="border border-pink-500/50 px-6 py-3 bg-pink-500/5 cyber-border-pink">
        <div class="text-xs text-pink-500/70 mb-1">DAY</div>
        <div class="text-xl text-[#ff00ff] neon-pink">{dayStr}</div>
      </div>
    </div>

    <!-- 进度条 -->
    <div class="max-w-2xl mx-auto space-y-3">
      <!-- 小时进度 -->
      <div class="relative">
        <div class="flex justify-between text-xs text-cyan-400/50 mb-1">
          <span>HOUR</span>
          <span>{Math.floor(hourProgress)}%</span>
        </div>
        <div class="h-1 bg-gray-900 rounded overflow-hidden border border-cyan-400/30 cyber-border-cyan-sm">
          <div 
            class="h-full bg-gradient-to-r from-cyan-400 to-cyan-600 transition-all duration-1000"
            style="width: {hourProgress}%"
          ></div>
        </div>
      </div>

      <!-- 分钟进度 -->
      <div class="relative">
        <div class="flex justify-between text-xs text-pink-500/50 mb-1">
          <span>MINUTE</span>
          <span>{Math.floor(minuteProgress)}%</span>
        </div>
        <div class="h-1 bg-gray-900 rounded overflow-hidden border border-pink-500/30 cyber-border-pink-sm">
          <div 
            class="h-full bg-gradient-to-r from-pink-500 to-pink-700 transition-all duration-1000"
            style="width: {minuteProgress}%"
          ></div>
        </div>
      </div>

      <!-- 秒进度 -->
      <div class="relative">
        <div class="flex justify-between text-xs text-purple-400/50 mb-1">
          <span>SECOND</span>
          <span>{Math.floor(secondProgress)}%</span>
        </div>
        <div class="h-1 bg-gray-900 rounded overflow-hidden border border-purple-400/30 cyber-border-purple-sm">
          <div 
            class="h-full bg-gradient-to-r from-purple-400 to-purple-600 transition-all duration-1000"
            style="width: {secondProgress}%"
          ></div>
        </div>
      </div>
    </div>

    <!-- 底部装饰 -->
    <div class="mt-12 flex items-center justify-center gap-4">
      <div class="h-px w-16 bg-gradient-to-r from-transparent to-cyan-400/50"></div>
      <div class="text-cyan-400/50 text-xs tracking-[0.2em]">
        [ CYBER CLOCK v0.1 ]
      </div>
      <div class="h-px w-16 bg-gradient-to-l from-transparent to-cyan-400/50"></div>
    </div>
  </div>

  <!-- 角落装饰 -->
  <div class="absolute top-4 left-4 w-16 h-16 border-t-2 border-l-2 border-cyan-400/50"></div>
  <div class="absolute top-4 right-4 w-16 h-16 border-t-2 border-r-2 border-cyan-400/50"></div>
  <div class="absolute bottom-4 left-4 w-16 h-16 border-b-2 border-l-2 border-cyan-400/50"></div>
  <div class="absolute bottom-4 right-4 w-16 h-16 border-b-2 border-r-2 border-cyan-400/50"></div>
</div>

<style>
  :global(body) {
    margin: 0;
    padding: 0;
    overflow: hidden;
  }

  .grid-bg {
    background-image: 
      linear-gradient(rgba(0, 255, 255, 0.1) 1px, transparent 1px),
      linear-gradient(90deg, rgba(0, 255, 255, 0.1) 1px, transparent 1px);
    background-size: 50px 50px;
    animation: pulse 2s ease-in-out infinite;
  }

  @keyframes pulse {
    0%, 100% { 
      opacity: 1; 
      transform: scale(1); 
    }
    50% { 
      opacity: 0.7; 
      transform: scale(0.98); 
    }
  }

  .scanline {
    background: linear-gradient(to bottom, transparent, rgba(0, 255, 255, 0.4), transparent);
    animation: scanline 8s linear infinite;
  }

  @keyframes scanline {
    0% { transform: translateY(-100%); }
    100% { transform: translateY(100vh); }
  }

  .noise-bg {
    background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 200 200' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noiseFilter'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23noiseFilter)'/%3E%3C/svg%3E");
    background-size: 200px 200px;
  }

  .glitch {
    animation: glitch 0.2s infinite;
  }

  @keyframes glitch {
    0% { transform: translate(0); }
    20% { transform: translate(-2px, 2px); }
    40% { transform: translate(-2px, -2px); }
    60% { transform: translate(2px, 2px); }
    80% { transform: translate(2px, -2px); }
    100% { transform: translate(0); }
  }

  .cyber-glow {
    text-shadow: 
      0 0 10px rgba(0, 255, 255, 0.8),
      0 0 20px rgba(0, 255, 255, 0.6),
      0 0 30px rgba(0, 255, 255, 0.4),
      0 0 40px rgba(0, 255, 255, 0.2);
  }

  .neon-pink {
    text-shadow: 
      0 0 10px rgba(255, 0, 255, 0.8),
      0 0 20px rgba(255, 0, 255, 0.6);
  }

  .cyber-border-cyan {
    box-shadow: 
      0 0 10px rgba(0, 255, 255, 0.5),
      inset 0 0 10px rgba(0, 255, 255, 0.2);
  }

  .cyber-border-pink {
    box-shadow: 
      0 0 10px rgba(255, 0, 255, 0.5),
      inset 0 0 10px rgba(255, 0, 255, 0.2);
  }

  .cyber-border-cyan-sm {
    box-shadow: 
      0 0 10px rgba(0, 255, 255, 0.5),
      inset 0 0 10px rgba(0, 255, 255, 0.2);
  }

  .cyber-border-pink-sm {
    box-shadow: 
      0 0 10px rgba(255, 0, 255, 0.5),
      inset 0 0 10px rgba(255, 0, 255, 0.2);
  }

  .cyber-border-purple-sm {
    box-shadow: 
      0 0 10px rgba(168, 85, 247, 0.5),
      inset 0 0 10px rgba(168, 85, 247, 0.2);
  }

  @media (max-width: 768px) {
    .cyber-glow {
      font-size: 3rem;
    }
  }
</style>
