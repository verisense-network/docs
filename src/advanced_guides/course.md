<style>
.course-header {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  padding: 2rem;
  margin: 3rem 0;
  border-radius: 12px;
  text-align: center;
}

.course-description {
  font-size: 1.1rem;
  line-height: 1.6;
  margin-bottom: 1rem;
}

.lesson-card {
  background: #1a1a2e;
  border: 1px solid #2d3748;
  border-radius: 12px;
  padding: 1.5rem;
  margin-bottom: 2rem;
  box-shadow: 0 4px 16px rgba(0,0,0,0.3);
  transition: transform 0.2s ease, box-shadow 0.2s ease;
}

.lesson-card:hover {
  transform: translateY(-2px);
  box-shadow: 0 8px 24px rgba(0,0,0,0.4);
  border-color: #4a5568;
}

.lesson-title {
  color: #e2e8f0;
  font-size: 2rem;
  font-weight: 600;
  margin-block-start: 1rem;
  margin-bottom: 0.5rem;
  border-bottom: 2px solid #667eea;
  padding-bottom: 0.5rem;
}

.lesson-objective {
  background: #2d3748;
  border-left: 4px solid #4299e1;
  padding: 1rem;
  margin: 1rem 0;
  border-radius: 0 8px 8px 0;
  font-style: italic;
  color: #cbd5e0;
}

.video-container {
  background: #000;
  border-radius: 8px;
  overflow: hidden;
  margin: 1.5rem 0;
  box-shadow: 0 4px 12px rgba(0,0,0,0.2);
}

.video-placeholder {
  background: linear-gradient(45deg, #2d3748 25%, transparent 25%), 
              linear-gradient(-45deg, #2d3748 25%, transparent 25%), 
              linear-gradient(45deg, transparent 75%, #2d3748 75%), 
              linear-gradient(-45deg, transparent 75%, #2d3748 75%);
  background-size: 20px 20px;
  background-position: 0 0, 0 10px, 10px -10px, -10px 0px;
  height: 450px;
  display: flex;
  align-items: center;
  justify-content: center;
  color: #a0aec0;
  font-size: 1.2rem;
  font-weight: 500;
}

.course-footer {
  background: #1a1a2e;
  border-top: 3px solid #667eea;
  padding: 2rem;
  margin-top: 3rem;
  text-align: center;
  border-radius: 12px;
  color: #e2e8f0;
}

.youtube-link {
  display: inline-flex;
  align-items: center;
  gap: 0.5rem;
  color: white;
  padding: 0.75rem 1.5rem;
  border-radius: 25px;
  text-decoration: none;
  font-weight: 600;
  transition: background 0.2s ease;
}
</style>

<div class="course-header">
  <h1 style="margin: 0; font-size: 2.5rem;">🚀 Verisense Developer Course</h1>
  <p class="course-description">
    The Verisense Developer Course is a hands-on series designed to help developers understand the core innovations of Verisense and learn how to build and deploy AI agents and agentic assets - including tools, context, domain-specific knowledge, and more - on the Verisense platform.
  </p>
</div>

<div class="lesson-card">
  <h3 class="lesson-title">
    Lesson 1: Verisense and the Active Blockchain Revolution
  </h3>
  <div class="lesson-objective">
    <strong>Objective:</strong> Understand the limitations of traditional blockchains and the core value and innovation of Verisense as an "Active Blockchain."
  </div>
  
  <div class="video-container">
    <iframe style="width: 100%;" height="450" src="https://www.youtube.com/embed/videoseries?si=_91luA-HoCygn8Lq&amp;list=PLVlQEN7bnh-lkU_R8m_P_y7BAhds1FqjV" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
  </div>
</div>

<div class="lesson-card">
  <h3 class="lesson-title">
    Lesson 2: A Deep Dive into Verisense Architecture: Validators, Monadring & Nucleus
  </h3>
  <div class="lesson-objective">
    <strong>Objective:</strong> Master the layered architecture of the Verisense network and understand the function and synergy of its core components.
  </div>
  
  <div class="video-container">
    <iframe style="width: 100%;" height="450" src="https://www.youtube.com/embed/WIPlW473vTM?si=NPWCtHJ4AumroTDH&amp;list=PLVlQEN7bnh-lkU_R8m_P_y7BAhds1FqjV" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
  </div>
</div>

<div class="lesson-card">
  <h3 class="lesson-title">
    Lesson 3: Hands-On: Creating and Deploying Your First Nucleus
  </h3>
  <div class="lesson-objective">
    <strong>Objective:</strong> Learn to use the Verisense SDK to write, compile, and deploy a simple Nucleus on-chain.
  </div>
  
  <div class="video-container">
    <iframe style="width: 100%;" height="450" src="https://www.youtube.com/embed/7c_1ZjJUhSI?si=PwFFyfyqJnA_dwAE&amp;list=PLVlQEN7bnh-lkU_R8m_P_y7BAhds1FqjV" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
  </div>
</div>

<div class="lesson-card">
  <h3 class="lesson-title">
    Lesson 4: Advanced Nucleus: State, Timers, TSS and External Communication
  </h3>
  <div class="lesson-objective">
    <strong>Objective:</strong> Master the core advanced features of a Nucleus: persistent storage, task scheduling, TSS and external HTTP requests.
  </div>
  
  <div class="video-container">
    <iframe style="width: 100%;" height="450" src="https://www.youtube.com/embed/mFRW6njduvw?si=PuFEfiYv9Zi30ej7&amp;list=PLVlQEN7bnh-lkU_R8m_P_y7BAhds1FqjV" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
  </div>
</div>

<div class="lesson-card">
  <h3 class="lesson-title">
    Lesson 5: Creating Your First AI Agent on Verisense
  </h3>
  <div class="lesson-objective">
    <strong>Objective:</strong> Synthesize all learned knowledge to build, register, and publish a simple AI Agent on the Verisense network.
  </div>
  
  <div class="video-container">
    <iframe style="width: 100%;" height="450" src="https://www.youtube.com/embed/5md1MSsKjkg?si=83-Vv7qjqRgUeUWR&amp;list=PLVlQEN7bnh-lkU_R8m_P_y7BAhds1FqjV" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
  </div>
</div>

<div class="course-footer">
  <p style="font-size: 1.1rem; margin-bottom: 1.5rem;">
    Welcome to the Verisense course tutorial series! Follow along with our comprehensive video guides.
  </p>
  
  <a href="https://www.youtube.com/@veri_sense" class="youtube-link" target="_blank">
    ▶️ Follow @Verisense Network on YouTube
  </a>
</div>