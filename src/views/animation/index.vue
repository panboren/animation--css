<template>
  <div class="home-container">
    <div class="animation-demo">
      <h1>CSS 动画演示</h1>
      <div class="preview-area">
        <div ref="animatedElement" :class="animationClass" class="animated-box">
          <div class="box-content">
            <span>{{ currentAnimationName }}</span>
          </div>
        </div>
      </div>
      <div class="info">
        <p>
          当前动画:
          <strong>{{ currentAnimationName }}</strong>
        </p>
        <p>
          使用方法:
          <code class="code">{{ usageExample }}</code>
        </p>
      </div>
      <div class="controls">
        <select v-model="selectedAnimation" class="animation-select" @change="triggerAnimation">
          <option value="">-- 选择动画 --</option>
          <optgroup label="V2 超级动画">
            <option value="ua-v2-hologram">🔮 全息投影</option>
            <option value="ua-v2-vortexIn">🌀 涡流吸入</option>
            <option value="ua-v2-sineWave">🌊 正弦波浪</option>
            <option value="ua-v2-origami">🎭 折纸效果</option>
            <option value="ua-v2-orbitIn">💫 星轨旋转</option>
            <option value="ua-v2-kaleidoscope">🔮 万花筒</option>
            <option value="ua-v2-nebulaExplosion">🌌 星云爆发</option>
            <option value="ua-v2-transformer">🤖 变形金刚</option>
            <option value="ua-v2-tidalWave">🌊 潮汐效果</option>
            <option value="ua-v2-magicTransform">🔮 魔术变换</option>
            <option value="ua-v2-dnaHelix">🧬 DNA双螺旋</option>
            <option value="ua-v2-quantumLeap">⚡ 量子跃迁</option>
            <option value="ua-v2-flameBurst">🔥 火焰喷射</option>
            <option value="ua-v2-aurora">🌌 极光效果</option>
            <option value="ua-v2-blackHole">🕳️ 黑洞吸入</option>
          </optgroup>

          <optgroup label="V3 超现实动画">
            <option value="ua-v3-wormhole">🌀 虫洞穿越</option>
            <option value="ua-v3-hologramPro">🔮 专业全息</option>
            <option value="ua-v3-quantumEntanglement">⚛️ 量子纠缠</option>
            <option value="ua-v3-neuralNetwork">🧠 神经网络</option>
            <option value="ua-v3-metaverse">🌐 元宇宙</option>
            <option value="ua-v3-vrImmersive">🥽 VR沉浸</option>
            <option value="ua-v3-warpDrive">🚀 曲速引擎</option>
            <option value="ua-v3-cyberpunk">🤖 赛博朋克</option>
            <option value="ua-v3-galaxyVortex">🌌 银河漩涡</option>
            <option value="ua-v3-nftReveal">🎨 NFT揭示</option>
            <option value="ua-v3-astralProjection">👻 灵魂出窍</option>
            <option value="ua-v3-timeTravel">⏰ 时间回溯</option>
            <option value="ua-v3-crystalBall">🔮 水晶球</option>
            <option value="ua-v3-bigBang">💥 宇宙大爆炸</option>
          </optgroup>

          <optgroup label="V5 极限突破动画">
            <option value="ua-v5-extremeVortex">🌀 极限漩涡</option>
            <option value="ua-v5-morphingKaleidoscope">🌈 变形万花筒</option>
            <option value="ua-v5-auroraPhantom">🌌 极光幻影</option>
            <option value="ua-v5-glassShatter">💎 玻璃破碎</option>
            <option value="ua-v5-dimensionTransit">🌀 维度穿越</option>
            <option value="ua-v5-liquidMorph">💧 液态变形</option>
            <option value="ua-v5-particleReassemble">✨ 粒子重组</option>
            <option value="ua-v5-spiralTime">🌀 时间螺旋</option>
            <option value="ua-v5-pixelCollapse">👾 像素坍塌</option>
            <option value="ua-v5-interstellar">🌌 星际穿越</option>
          </optgroup>

          <optgroup label="V6 传奇动画">
            <option value="ua-v6-lightShadow">🌟 光影穿梭</option>
            <option value="ua-v6-spaceFold">🎭 空间折叠</option>
            <option value="ua-v6-crystalFission">💎 晶体裂变</option>
            <option value="ua-v6-electromagneticStorm">⚡ 电磁风暴</option>
            <option value="ua-v6-quantumRipples">🌊 量子涟漪</option>
            <option value="ua-v6-dimensionGate">🔮 维度之门</option>
            <option value="ua-v6-auroraSpectrum">🌈 极光光谱</option>
            <option value="ua-v6-stardustAssembly">💫 星尘聚合</option>
            <option value="ua-v6-rainbowFission">🌈 彩虹裂变</option>
            <option value="ua-v6-lightningPulse">⚡ 闪电脉冲</option>
            <option value="ua-v6-singularityExplosion">🔮 奇点爆炸</option>
            <option value="ua-v6-deepSpace">🌌 深空穿梭</option>
          </optgroup>
        </select>

        <button class="play-btn" @click="triggerAnimation">播放</button>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed, nextTick } from 'vue'

const selectedAnimation = ref('')
const animatedElement = ref<HTMLElement>()
const isAnimating = ref(false)

const animationClass = computed(() => {
  if (!selectedAnimation.value) return ''
  // 根据动画版本添加对应的基础类
  const version = selectedAnimation.value.match(/ua-v(\d+)/)?.[1]
  const baseClass = version ? `ua-v${version}-animated` : ''
  return `${baseClass} ${selectedAnimation.value}`.trim()
})

const currentAnimationName = computed(() => {
  if (!selectedAnimation.value) return '请选择一个动画'
  const select = document.querySelector('.animation-select') as HTMLSelectElement
  const option = select?.options[select.selectedIndex]
  return option?.text || selectedAnimation.value
})

const usageExample = computed(() => {
  if (!selectedAnimation.value) return 'class="ua-animated [动画名]"'
  return `class="${animationClass.value}"`
})

const triggerAnimation = async () => {
  if (!selectedAnimation.value) return

  // 移除动画类
  isAnimating.value = false

  // 强制重绘
  await nextTick()
  animatedElement.value?.offsetWidth

  // 重新添加动画
  isAnimating.value = true
}
</script>

<style lang="scss" scoped>
/* 使用原始完整动画库 - 包含所有版本的完整 @keyframes 定义 */
@import './animation.css';

.home-container {
  width: 100vw;
  height: 100vh;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 20px;
}

.animation-demo {
  background: rgba(255, 255, 255, 0.95);
  border-radius: 20px;
  padding: 40px;
  box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);
  max-width: 800px;
  width: 100%;
  text-align: center;
}

h1 {
  color: #333;
  margin-bottom: 30px;
  font-size: 2rem;
}

.controls {
  display: flex;
  gap: 15px;
  justify-content: center;
  margin-bottom: 40px;
  flex-wrap: wrap;
}

.animation-select {
  padding: 12px 20px;
  font-size: 16px;
  border: 2px solid #667eea;
  border-radius: 10px;
  background: white;
  color: #333;
  cursor: pointer;
  min-width: 300px;
  transition: all 0.3s ease;

  &:hover {
    border-color: #764ba2;
  }

  &:focus {
    outline: none;
    box-shadow: 0 0 0 3px rgba(102, 126, 234, 0.2);
  }

  optgroup {
    font-weight: bold;
    background: #f0f0f0;
  }

  option {
    padding: 10px;
  }
}

.play-btn {
  padding: 12px 30px;
  font-size: 16px;
  font-weight: 600;
  color: white;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  border: none;
  border-radius: 10px;
  cursor: pointer;
  transition: all 0.3s ease;

  &:hover {
    transform: translateY(-2px);
    box-shadow: 0 5px 20px rgba(102, 126, 234, 0.4);
  }

  &:active {
    transform: translateY(0);
  }
}

.preview-area {
  height: 300px;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-bottom: 30px;
}

.animated-box {
  width: 200px;
  height: 200px;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  border-radius: 20px;
  display: flex;
  align-items: center;
  justify-content: center;
  color: white;
  font-size: 18px;
  font-weight: 600;
  box-shadow: 0 10px 30px rgba(102, 126, 234, 0.4);
  transition: all 0.3s ease;
}

.box-content {
  text-align: center;
  padding: 20px;
}

.info {
  background: #f8f9fa;
  padding: 20px;
  border-radius: 10px;
  text-align: left;

  p {
    margin: 10px 0;
    color: #555;
    font-size: 14px;
  }

  strong {
    color: #667eea;
  }
}

.code {
  background: #2d2d2d;
  color: #f8f8f2;
  padding: 4px 8px;
  border-radius: 4px;
  font-family: 'Courier New', monospace;
  font-size: 13px;
}
</style>
