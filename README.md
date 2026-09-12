import React, { useState, useEffect } from 'react';

// Dados estruturados baseados no PDF "LIVRO- Como criar Servidores Mágicos"
const STEPS = [
  {
    id: 'proposito',
    title: '1. Propósito',
    description: 'Para que ele foi criado? Eles podem ser usados para absolutamente qualquer coisa, desde que existam vias de manifestação. Seja criativo, mas mantenha os pés no chão.',
    placeholder: 'Ex: Você irá me ajudar a lançar um hit de sucesso nas plataformas de streaming...',
    type: 'textarea'
  },
  {
    id: 'nome',
    title: '2. Nome',
    description: 'Um nome é mais do que uma forma bacana de se chamar algo, os nomes têm significados e podem carregar influência.',
    placeholder: 'Ex: Zorox',
    type: 'text'
  },
  {
    id: 'aparencia',
    title: '3. Aparência',
    description: 'Como ele se parece? Use todos os seus sentidos. Aconselha-se pensar em coisas que fogem da realidade ordinária para ter significado exclusivo para você.',
    placeholder: 'Ex: Um velho árabe de pele morena, túnica azul celeste e olhos roxos...',
    type: 'textarea'
  },
  {
    id: 'viaManifestacao',
    title: '4. Via de Manifestação',
    description: 'Como ele agirá no mundo físico? Sem uma via de manifestação, a magia não acontece. Adeque o intento para que seja viável.',
    placeholder: 'Ex: Me ajudar a buscar referências criativas para as composições...',
    type: 'textarea'
  },
  {
    id: 'morte',
    title: '5. Morte',
    description: 'Servidores não devem ser criados para serem eternos. Defina quando ou como a existência dele se encerra.',
    placeholder: 'Ex: Você viverá até que cumpra seu propósito ou até eu ordenar sua morte.',
    type: 'text'
  },
  {
    id: 'chamado',
    title: '6. Chamado',
    description: 'É a forma pela qual você chamará seu servidor. Pode ser o nome dele seguido de uma palavra específica.',
    placeholder: 'Ex: Urú',
    type: 'text'
  },
  {
    id: 'alimentacao',
    title: '7. Alimentação',
    description: 'O que ele receberá em troca do serviço? Pense na alimentação como um pagamento (velas, incensos, ações, meditações).',
    placeholder: 'Ex: Queimarei uma vela azul ou te oferecerei uma maçã com mel.',
    type: 'text'
  },
  {
    id: 'comunicacao',
    title: '8. Comunicação',
    description: 'Como vocês irão se comunicar para saber se o trabalho flui? Tarô, sonhos, intuição, etc.',
    placeholder: 'Ex: Através do Tarô e de sonhos lúcidos.',
    type: 'text'
  },
  {
    id: 'ancora',
    title: '9. Âncora Física',
    description: 'O que vai ligar a existência etérea do servidor ao mundo físico (ex: uma pedra, anel, desenho).',
    placeholder: 'Ex: Este anel de ouro / Este cristal de ametista.',
    type: 'text'
  },
  {
    id: 'palavraFatal',
    title: '10. Palavra Fatal',
    description: 'Um botão de emergência. Comando para que a vida do servo se dissipe imediatamente.',
    placeholder: 'Ex: Fobos Tenebris est',
    type: 'text'
  },
  {
    id: 'sigilo',
    title: '11. Sigilo',
    description: 'A assinatura energética do servidor e canal de evocação. Ferramenta integrada de Sigilos Planetários baseada nos quadrados mágicos (Kameas).',
    placeholder: '',
    type: 'custom_sigil'
  }
];

const PLANETS = {
  sol: {
    name:'Sol', sym:'☉', n:6, M:111, S:666,
    keywords:'vitalidade, carisma, visibilidade, iluminação',
    description: 'Representa a identidade, o ego, a vitalidade e a essência do self. Está ligado ao propósito de vida, à liderança e à energia criativa.',
    grid:[6,32,3,34,35,1, 7,11,27,28,8,30, 19,14,16,15,23,24, 18,20,22,21,17,13, 25,29,10,9,26,12, 36,5,33,4,2,31]
  },
  lua: {
    name:'Lua', sym:'☾', n:9, M:369, S:3321,
    keywords:'intuição, sonhos, fluidez, inconsciente',
    description: 'Rege as emoções, o inconsciente, os hábitos e os instintos. Está associada à maternidade, à memória, ao lar e à forma como reagimos emocionalmente.',
    grid:[5,37,78,29,70,21,62,13,54, 6,38,79,30,71,22,63,14,46, 47,7,39,80,31,72,23,55,15, 16,48,8,40,81,32,64,24,56, 57,17,49,9,41,73,33,65,25, 26,58,18,50,1,42,74,34,66, 67,27,59,10,51,2,43,75,35, 36,68,19,60,11,52,3,44,76, 77,28,69,20,61,12,53,4,45]
  },
  mercurio: {
    name:'Mercúrio', sym:'☿', n:8, M:260, S:2080,
    keywords:'mente, comunicação, negociação, lógica',
    description: 'Governa a comunicação, o intelecto, a lógica e o aprendizado. Rege as trocas de informação, as viagens curtas e o raciocínio rápido.',
    grid:[8,58,59,5,4,62,63,1, 49,15,14,52,53,11,10,56, 41,23,22,44,45,19,18,48, 32,34,35,29,28,38,39,25, 40,26,27,37,36,30,31,33, 17,47,46,20,21,43,42,24, 9,55,54,12,13,51,50,16, 64,2,3,61,60,6,7,57]
  },
  venus: {
    name:'Vênus', sym:'♀', n:7, M:175, S:1225,
    keywords:'amor, harmonia, beleza, reconciliação',
    description: 'Rege o amor, a beleza, os relacionamentos e os valores financeiros. Está ligada à harmonia, à atração, à arte e ao prazer.',
    grid:[22,47,16,41,10,35,4, 5,23,48,17,42,11,29, 30,6,24,49,18,36,12, 13,31,7,25,43,19,37, 38,14,32,1,26,44,20, 21,39,8,33,2,27,45, 46,15,40,9,34,3,28]
  },
  marte: {
    name:'Marte', sym:'♂', n:5, M:65, S:325,
    keywords:'coragem, defesa, ímpeto, determinação',
    description: 'Representa a ação, a coragem, a paixão e a agressividade. Rege a força física, o impulso sexual, o espírito competitivo e a determinação.',
    grid:[11,24,7,20,3, 4,12,25,8,16, 17,5,13,21,9, 10,18,1,14,22, 23,6,19,2,15]
  },
  jupiter: {
    name:'Júpiter', sym:'♃', n:4, M:34, S:136,
    keywords:'expansão, prosperidade, favor, autoridade',
    description: 'Governa a expansão, a sorte, a sabedoria e a filosofia. Está associado ao crescimento pessoal, à generosidade, à fé e às grandes oportunidades.',
    grid:[4,14,15,1, 9,7,6,12, 5,11,10,8, 16,2,3,13]
  },
  saturno: {
    name:'Saturno', sym:'♄', n:3, M:15, S:45,
    keywords:'proteção, limites, contenção, disciplina',
    description: 'Representa a disciplina, a estrutura, o tempo e as responsabilidades. Rege os limites, as lições de vida, O amadurecimento e as restrições.',
    grid:[4,9,2, 3,5,7, 8,1,6]
  }
};

function buildPosMap(grid, n){
  const map = {};
  for(let r=0;r<n;r++) for(let c=0;c<n;c++) map[grid[r*n+c]] = {r,c};
  return map;
}

function textToLetters(text, condense){
  let clean = text.normalize('NFD').replace(/[\u0300-\u036f]/g,'').toUpperCase().replace(/[^A-Z]/g,'');
  if(condense){
    let out = '';
    for(const ch of clean) { if(ch !== out[out.length-1]) out += ch; }
    clean = out;
  }
  return clean.split('');
}

function digitalRoot(x){
  while(x > 9){ x = String(x).split('').reduce((a,d)=>a+Number(d),0); }
  return x;
}

function lettersToValues(letters, planetKey, n){
  const capacity = n*n;
  const seen = {};
  return letters.map(ch=>{
    const order = ch.charCodeAt(0) - 64;
    const occ = seen[ch] || 0;
    seen[ch] = occ + 1;

    if(planetKey === 'saturno'){
      return digitalRoot(order + 27*occ);
    }
    if(capacity > 27){
      const extended = order + 27*occ;
      if(extended <= capacity) return extended;
      return ((extended - 1) % capacity) + 1;
    }
    return ((order - 1) % capacity) + 1;
  });
}

function dedupeByValue(pairs){
  const seenValues = new Set();
  const result = [];
  pairs.forEach(p=>{
    if(!seenValues.has(p.value)){
      seenValues.add(p.value);
      result.push(p);
    }
  });
  return result;
}

function selectFinalPairs(pairs){
  if(pairs.length <= 9) return { result: pairs.slice(), wasRandom: false };

  const seen = new Set();
  const firstIdx = [], repeatIdx = [];
  pairs.forEach((p,i)=>{
    if(!seen.has(p.letter)){ seen.add(p.letter); firstIdx.push(i); }
    else { repeatIdx.push(i); }
  });

  const toRemoveCount = pairs.length - 9;
  let keep, wasRandom;

  if(repeatIdx.length >= toRemoveCount){
    let pool = repeatIdx.slice();
    for(let i=0;i<toRemoveCount;i++){
      pool.splice(Math.floor(Math.random()*pool.length), 1);
    }
    keep = new Set([...firstIdx, ...pool]);
    wasRandom = repeatIdx.length > toRemoveCount;
  } else {
    const stillNeed = toRemoveCount - repeatIdx.length;
    let pool = firstIdx.slice();
    for(let i=0;i<stillNeed;i++){
      pool.splice(Math.floor(Math.random()*pool.length), 1);
    }
    keep = new Set(pool);
    wasRandom = true;
  }

  return { result: pairs.filter((p,i)=>keep.has(i)), wasRandom };
}

function buildPathD(points, t){
  if(points.length < 2) return '';
  if(points.length === 2) return `M ${points[0].x} ${points[0].y} L ${points[1].x} ${points[1].y}`;
  if(t === 0) {
    let d = `M ${points[0].x} ${points[0].y} `;
    for(let i=1; i<points.length; i++) {
      d += `L ${points[i].x} ${points[i].y} `;
    }
    return d;
  }
  let d = `M ${points[0].x} ${points[0].y} `;
  for(let i=0;i<points.length-1;i++){
    const p0 = points[i-1] || points[i], p1 = points[i], p2 = points[i+1], p3 = points[i+2] || p2;
    const cp1x = p1.x + (p2.x - p0.x)/6, cp1y = p1.y + (p2.y - p0.y)/6;
    const cp2x = p2.x - (p3.x - p1.x)/6, cp2y = p2.y - (p3.y - p1.y)/6;
    d += `C ${p1.x + t*(cp1x - p1.x)} ${p1.y + t*(cp1y - p1.y)} ${p2.x + t*(cp2x - p2.x)} ${p2.y + t*(cp2y - p2.y)} ${p2.x} ${p2.y} `;
  }
  return d;
}

function generateSVGMarkup(planet, pairs, showNumbers, smoothing, printMode = false, cleanHome = false){
  const n = planet.n, size = 600, margin = 100, inner = size - margin*2, cell = inner / n;
  const posMap = buildPosMap(planet.grid, n);
  const centerOf = (val) => ({ x: margin + (posMap[val].c+0.5)*cell, y: margin + (posMap[val].r+0.5)*cell });
  
  let points = pairs.map(p=>centerOf(p.value));
  points = points.filter((p,i)=> i===0 || p.x!==points[i-1].x || p.y!==points[i-1].y);

  // Define a cor de fundo do sigilo (transparente ou da mesma cor do papel no modo impressão)
  const bgColor = printMode ? "#f0e4c8" : "#0f172a";
  const lineColor = printMode ? "#2c1d0c" : "#d8b4fe";
  const gridColor = printMode ? "#cbd5e1" : "#1e293b";
  const textColor = printMode ? "#64748b" : "#475569";

  let svg = `<svg viewBox="0 0 ${size} ${size}" xmlns="http://www.w3.org/2000/svg">`;
  svg += `<rect width="${size}" height="${size}" fill="${printMode ? 'transparent' : bgColor}" ${cleanHome ? 'fill-opacity="0"' : ''}/>`; 
  
  // O círculo maior fica visível no modo de criação e no Contrato Final
  if (!cleanHome) {
    const radius = Math.hypot(inner/2, inner/2);
    svg += `<circle cx="${size/2}" cy="${size/2}" r="${radius}" fill="none" stroke="${lineColor}" stroke-width="2" opacity="${printMode ? 0.8 : 0.3}"/>`; 
  }

  // Ocultar APENAS a grade e os números se for para o Contrato Final (printMode)
  if (!cleanHome && !printMode) {
    for(let i=0;i<=n;i++){
      const pos = margin + i*cell;
      svg += `<line x1="${pos}" y1="${margin}" x2="${pos}" y2="${margin+inner}" stroke="${gridColor}" stroke-width="2"/>`; 
      svg += `<line x1="${margin}" y1="${pos}" x2="${margin+inner}" y2="${pos}" stroke="${gridColor}" stroke-width="2"/>`;
    }

    if(showNumbers){
      const fs = Math.max(10, Math.min(cell*0.28, 20));
      for(let r=0;r<n;r++){
        for(let c=0;c<n;c++){
          svg += `<text x="${margin + (c+0.5)*cell}" y="${margin + (r+0.5)*cell + fs*0.35}" font-size="${fs}" fill="${textColor}" opacity="0.8" text-anchor="middle" font-family="sans-serif">${planet.grid[r*n+c]}</text>`;
        }
      }
    }
  }

  if(points.length >= 2){
    svg += `<path d="${buildPathD(points, smoothing)}" fill="none" stroke="${lineColor}" stroke-width="5" stroke-linecap="round" stroke-linejoin="round"/>`;
  }

  if(points.length >= 1){
    // O círculo inicial usa a cor de fundo local para sobrepor a linha do sigilo
    svg += `<circle cx="${points[0].x}" cy="${points[0].y}" r="8" fill="${points.length === 1 ? lineColor : bgColor}" stroke="${lineColor}" stroke-width="4"/>`;
  }

  if(!cleanHome && points.length >= 2){
    const pLast = points[points.length-1], pPrev = points[points.length-2];
    const len = Math.hypot(pLast.x - pPrev.x, pLast.y - pPrev.y) || 1;
    const ux = (pLast.x - pPrev.x)/len, uy = (pLast.y - pPrev.y)/len;
    const tickLen = 14;
    svg += `<line x1="${pLast.x - uy*tickLen}" y1="${pLast.y + ux*tickLen}" x2="${pLast.x + uy*tickLen}" y2="${pLast.y - ux*tickLen}" stroke="${lineColor}" stroke-width="5" stroke-linecap="round"/>`;
  }
  return svg + `</svg>`;
}

export default function AstralServitorApp() {
  const [currentStep, setCurrentStep] = useState(-1);
  const [activeView, setActiveView] = useState('creator'); // 'creator', 'grimoire', 'view_contract'
  const [savedServitors, setSavedServitors] = useState([]);
  const [viewingServitor, setViewingServitor] = useState(null);

  const [formData, setFormData] = useState({
    proposito: '',
    nome: '',
    aparencia: '',
    viaManifestacao: '',
    morte: '',
    chamado: '',
    alimentacao: '',
    comunicacao: '',
    ancora: '',
    palavraFatal: '',
    sigilo: ''
  });

  const [sigilPlanet, setSigilPlanet] = useState('sol');
  const [sigilIntention, setSigilIntention] = useState('');
  const [isIntentionTouched, setIsIntentionTouched] = useState(false);
  const [sigilOptNumbers, setSigilOptNumbers] = useState(true);
  const [sigilSmooth, setSigilSmooth] = useState(40);
  
  const [tempSigilMeta, setTempSigilMeta] = useState(null);
  const [sigilMeta, setSigilMeta] = useState(null);
  const [sigilCache, setSigilCache] = useState({});

  const [isSidebarOpen, setIsSidebarOpen] = useState(false);

  const [generatedImage, setGeneratedImage] = useState(null);
  const [isGeneratingImage, setIsGeneratingImage] = useState(false);

  // Estados do Contrato Final
  const [isEditingText, setIsEditingText] = useState(false);
  const [showSaveDropdown, setShowSaveDropdown] = useState(false);
  const [contractCustomText, setContractCustomText] = useState(null);

  const handleInputChange = (field, value) => {
    setFormData(prev => ({ ...prev, [field]: value }));
  };

  const handleSaveAction = () => {
    setShowSaveDropdown(false);
    window.print();
  };

  const nextStep = () => {
    if (currentStep < STEPS.length) setCurrentStep(curr => curr + 1);
  };

  const prevStep = () => {
    if (currentStep > -1) setCurrentStep(curr => curr - 1);
  };

  const handleGenerateImage = async () => {
    if (!formData.aparencia.trim()) {
      alert("Por favor, descreva a aparência antes de gerar a imagem.");
      return;
    }

    setIsGeneratingImage(true);
    try {
      const userPrompt = `Crie uma ilustração vertical 9:16 de um servidor mágico astral (forma-pensamento) com a seguinte aparência: ${formData.aparencia}. Estilo de ilustração de fantasia, místico, ocultista, arte digital altamente detalhada e imersiva. Fundo claro, estilo pergaminho ou neutro.`;
      const payload = {
          contents: [{ parts: [{ text: userPrompt }] }],
          generationConfig: { responseModalities: ['TEXT', 'IMAGE'], imageConfig: { aspectRatio: "9:16" } },
      };
      
      const apiKey = "";
      const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-3.1-flash-image:generateContent?key=${apiKey}`;
      
      const response = await fetch(apiUrl, {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify(payload)
      });
      const result = await response.json();
      
      if(result.error) {
        throw new Error(result.error.message || "Erro desconhecido na API");
      }

      const base64Data = result?.candidates?.[0]?.content?.parts?.find(p => p.inlineData)?.inlineData?.data;
      
      if (base64Data) {
        setGeneratedImage(`data:image/png;base64,${base64Data}`);
      } else {
        alert("Não foi possível extrair a imagem da resposta.");
      }
    } catch (error) {
      console.error("Erro ao gerar imagem:", error);
      alert(`Falha ao gerar imagem: ${error.message}`);
    } finally {
      setIsGeneratingImage(false);
    }
  };

  const handleDownloadImagePDF = async () => {
    if (!generatedImage) return;
    const fileName = window.prompt("Nome do arquivo do PDF da imagem:", `Aparencia_${formData.nome || 'Servidor'}`);
    if (!fileName) return;

    try {
      await loadScript('https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js');
      const { jsPDF } = window.jspdf;
      const doc = new jsPDF();
      doc.addImage(generatedImage, 'PNG', 15, 20, 180, 180 * (16/9));
      doc.save(fileName.endsWith('.pdf') ? fileName : `${fileName}.pdf`);
    } catch (e) {
      alert("Erro ao baixar PDF da imagem. Tente novamente.");
    }
  };

  const handleSaveToGrimoire = () => {
    const newServitor = {
      id: Date.now(),
      date: new Date().toLocaleDateString(),
      formData: { ...formData },
      sigilMeta: sigilMeta,
      generatedImage: generatedImage,
      customText: contractCustomText
    };
    setSavedServitors(prev => [...prev, newServitor]);
    alert("Servidor salvo no Grimório com sucesso!");
    setShowSaveDropdown(false);
    setActiveView('grimoire');
  };

  const loadScript = (src) => {
    return new Promise((resolve, reject) => {
      if (document.querySelector(`script[src="${src}"]`)) {
        resolve();
        return;
      }
      const script = document.createElement('script');
      script.src = src;
      script.onload = resolve;
      script.onerror = reject;
      document.head.appendChild(script);
    });
  };

  useEffect(() => {
    if (currentStep >= 0 && currentStep < STEPS.length && STEPS[currentStep].id === 'sigilo' && !isIntentionTouched && !sigilIntention && formData.proposito) {
      setSigilIntention(formData.proposito);
    }
  }, [currentStep, sigilIntention, formData.proposito, isIntentionTouched]);

  const handleGenerateSigil = () => {
    if (!sigilIntention.trim()) return;
    
    const letters = textToLetters(sigilIntention, false);
    if(letters.length === 0) return;

    const planet = PLANETS[sigilPlanet];
    const values = lettersToValues(letters, sigilPlanet, planet.n);
    const rawPairs = letters.map((l, i) => ({ letter: l, value: values[i] }));
    const deduped = dedupeByValue(rawPairs);
    
    const cacheKey = sigilIntention.trim().toUpperCase() + '_' + sigilPlanet;
    let finalPairs;

    if (deduped.length <= 9) {
      finalPairs = deduped;
    } else if (sigilCache[cacheKey]) {
      finalPairs = sigilCache[cacheKey];
    } else {
      finalPairs = selectFinalPairs(deduped).result;
      setSigilCache(prev => ({ ...prev, [cacheKey]: finalPairs }));
    }

    setTempSigilMeta({ finalPairs, planetKey: sigilPlanet });
  };

  const handleConfirmSigil = () => {
    setSigilMeta({ ...tempSigilMeta, smooth: sigilSmooth });
    handleInputChange('sigilo', 'gerado');
  };

  const liveSigilSvg = tempSigilMeta ? generateSVGMarkup(PLANETS[tempSigilMeta.planetKey], tempSigilMeta.finalPairs, sigilOptNumbers, sigilSmooth / 100, false) : '';
  
  // Variáveis para unificar o Contrato (seja criando um novo ou visualizando um do grimório)
  const isShowingContract = (activeView === 'creator' && currentStep === STEPS.length) || activeView === 'view_contract';
  const contractData = activeView === 'view_contract' && viewingServitor ? viewingServitor.formData : formData;
  const contractImage = activeView === 'view_contract' && viewingServitor ? viewingServitor.generatedImage : generatedImage;
  const contractSigilMeta = activeView === 'view_contract' && viewingServitor ? viewingServitor.sigilMeta : sigilMeta;
  const contractText = activeView === 'view_contract' && viewingServitor ? viewingServitor.customText : contractCustomText;
  
  const contractSigilSvg = contractSigilMeta ? generateSVGMarkup(PLANETS[contractSigilMeta.planetKey], contractSigilMeta.finalPairs, false, contractSigilMeta.smooth / 100, true) : '';

  const orbitingSymbols = [
    { sym: '♄', label: 'Saturno' },
    { sym: '☯', label: 'Yin Yang' },
    { sym: '☿', label: 'Mercúrio' },
    { sym: '🜔', label: 'Alquimia' },
    { sym: '♃', label: 'Júpiter' },
    { sym: '♆', label: 'Netuno' },
    { sym: '♂', label: 'Marte' },
    { sym: '☉', label: 'Sol' },
    { sym: '♀', label: 'Vênus' },
    { sym: '☽', label: 'Lua' },
    { sym: '🜁', label: 'Ar' },
    { sym: '🜂', label: 'Fogo' }
  ];

  return (
    <div className="min-h-screen bg-slate-950 text-slate-200 font-sans print:bg-white print:text-black flex flex-col">
      
      <style>{`
        @import url('https://fonts.googleapis.com/css2?family=Cinzel:wght@700&family=Dancing+Script:wght@600&display=swap');
        
        @keyframes spinRing {
          from { transform: rotate(0deg); }
          to { transform: rotate(360deg); }
        }
        @keyframes orbitSymbols {
          from { transform: rotate(0deg); }
          to { transform: rotate(-360deg); }
        }
        @keyframes keepUpright {
          from { transform: rotate(0deg); }
          to { transform: rotate(360deg); }
        }
        .animate-spin-ring {
          animation: spinRing 30s linear infinite;
        }
        .animate-orbit-symbols {
          animation: orbitSymbols 35s linear infinite;
        }
        .animate-keep-upright {
          animation: keepUpright 35s linear infinite;
        }
        .font-cursive-contract {
          font-family: 'Dancing Script', cursive;
        }
        .font-cinzel {
          font-family: 'Cinzel', serif;
        }
      `}</style>

      {/* SIDEBAR DRAWER (ICONS COM CORES NEUTRAS) */}
      <div className={`fixed inset-0 z-50 transition-opacity duration-300 print:hidden ${isSidebarOpen ? 'opacity-100 pointer-events-auto' : 'opacity-0 pointer-events-none'}`}>
        <div className="absolute inset-0 bg-black/70 backdrop-blur-sm" onClick={() => setIsSidebarOpen(false)}></div>

        <div className={`absolute top-0 left-0 bottom-0 w-80 max-w-[85vw] bg-slate-900 border-r border-slate-800 shadow-2xl flex flex-col transition-transform duration-300 transform ${isSidebarOpen ? 'translate-x-0' : '-translate-x-full'}`}>
          <div className="p-5 border-b border-slate-800 flex justify-between items-center bg-slate-950">
            <h2 className="text-lg font-bold text-slate-300 flex items-center gap-2.5 font-cinzel">
              <svg className="w-5 h-5 text-slate-400" fill="none" stroke="currentColor" strokeWidth="2" viewBox="0 0 24 24"><path strokeLinecap="round" strokeLinejoin="round" d="M4 6h16M4 12h16M4 18h7" /></svg>
              Menu de Etapas
            </h2>
            <button 
              onClick={() => setIsSidebarOpen(false)}
              className="text-slate-400 hover:text-white p-1 rounded-lg hover:bg-slate-800 transition-colors"
            >
              <svg className="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path strokeLinecap="round" strokeLinejoin="round" strokeWidth="2" d="M6 18L18 6M6 6l12 12" /></svg>
            </button>
          </div>

          <div className="flex-1 overflow-y-auto p-4 space-y-2">
            <button
              onClick={() => {
                setActiveView('creator');
                setCurrentStep(-1);
                setIsSidebarOpen(false);
              }}
              className={`w-full text-left px-4 py-3 rounded-xl text-sm font-medium transition-all flex items-center gap-3 border ${
                activeView === 'creator' && currentStep === -1 
                  ? 'bg-slate-800 text-slate-100 border-slate-600' 
                  : 'bg-slate-950/40 text-slate-400 border-slate-800/80 hover:bg-slate-800/50 hover:text-slate-200'
              }`}
            >
              <svg className="w-4 h-4 text-slate-400" fill="none" stroke="currentColor" strokeWidth="2" viewBox="0 0 24 24"><path strokeLinecap="round" strokeLinejoin="round" d="M3 12l2-2m0 0l7-7 7 7M5 10v10a1 1 0 001 1h3m10-11l2 2m-2-2v10a1 1 0 01-1 1h-3m-6 0a1 1 0 001-1v-4a1 1 0 011-1h2a1 1 0 011 1v4a1 1 0 001 1m-6 0h6" /></svg>
              <span>Página Inicial</span>
            </button>

            <button
              onClick={() => {
                setActiveView('grimoire');
                setIsSidebarOpen(false);
              }}
              className={`w-full text-left px-4 py-3 rounded-xl text-sm font-medium transition-all flex items-center gap-3 border ${
                activeView === 'grimoire' || activeView === 'view_contract'
                  ? 'bg-slate-800 text-slate-100 border-slate-600' 
                  : 'bg-slate-950/40 text-slate-400 border-slate-800/80 hover:bg-slate-800/50 hover:text-slate-200'
              }`}
            >
              <svg className="w-4 h-4 text-slate-400" fill="none" stroke="currentColor" strokeWidth="2" viewBox="0 0 24 24"><path strokeLinecap="round" strokeLinejoin="round" d="M12 6.253v13m0-13C10.832 5.477 9.246 5 7.5 5S4.168 5.477 3 6.253v13C4.168 18.477 5.754 18 7.5 18s3.332.477 4.5 1.253m0-13C13.168 5.477 14.754 5 16.5 5c1.747 0 3.332.477 4.5 1.253v13C19.832 18.477 18.247 18 16.5 18c-1.746 0-3.332.477-4.5 1.253" /></svg>
              <span>Grimório</span>
            </button>

            <span className="block text-xs font-semibold text-slate-500 uppercase tracking-wider my-3 px-2 mt-6">
              Etapas da Forja:
            </span>

            {STEPS.map((s, idx) => {
              const isFilled = formData[s.id] && formData[s.id].trim() !== '';
              const isActive = activeView === 'creator' && currentStep === idx;
              return (
                <button
                  key={s.id}
                  onClick={() => {
                    setActiveView('creator');
                    setCurrentStep(idx);
                    setIsSidebarOpen(false);
                  }}
                  className={`w-full text-left px-4 py-3 rounded-xl text-sm font-medium transition-all flex items-center justify-between border ${
                    isActive 
                      ? 'bg-slate-800 text-slate-100 border-slate-600' 
                      : isFilled
                      ? 'bg-slate-900/60 text-slate-300 border-slate-800'
                      : 'bg-slate-950/40 text-slate-400 border-slate-800/80 hover:bg-slate-800/50 hover:text-slate-200'
                  }`}
                >
                  <div className="flex items-center gap-3">
                    <svg className={`w-4 h-4 ${isFilled ? 'text-slate-300' : 'text-slate-600'}`} fill="none" stroke="currentColor" strokeWidth="2" viewBox="0 0 24 24"><path strokeLinecap="round" strokeLinejoin="round" d="M9 12h6m-6 4h6m2 5H7a2 2 0 01-2-2V5a2 2 0 012-2h5.586a1 1 0 01.707.293l5.414 5.414a1 1 0 01.293.707V19a2 2 0 01-2 2z" /></svg>
                    <span>{s.title}</span>
                  </div>
                  {isFilled && <span className="text-[10px] text-slate-400 font-semibold tracking-wider uppercase">Concluído</span>}
                </button>
              );
            })}

            <div className="pt-2">
              <button
                onClick={() => {
                  setActiveView('creator');
                  setCurrentStep(STEPS.length);
                  setIsSidebarOpen(false);
                }}
                className={`w-full text-left px-4 py-3 rounded-xl text-sm font-medium transition-all flex items-center gap-3 border ${
                  activeView === 'creator' && currentStep === STEPS.length 
                    ? 'bg-slate-800 text-slate-100 border-slate-600' 
                    : 'bg-slate-950/40 text-slate-400 border-slate-800/80 hover:bg-slate-800/50 hover:text-slate-200'
                }`}
              >
                <svg className="w-4 h-4 text-slate-400" fill="none" stroke="currentColor" strokeWidth="2" viewBox="0 0 24 24"><path strokeLinecap="round" strokeLinejoin="round" strokeWidth="2" d="M9 5H7a2 2 0 00-2 2v12a2 2 0 002 2h10a2 2 0 002-2V7a2 2 0 00-2-2h-2M9 5a2 2 0 002 2h2a2 2 0 002-2M9 5a2 2 0 012-2h2a2 2 0 012 2m-6 9l2 2 4-4" /></svg>
                <span>Contrato Final</span>
              </button>
            </div>
          </div>
        </div>
      </div>

      {/* HEADER */}
      <header className="print:hidden bg-slate-900 border-b border-purple-900/50 p-4 md:p-6 shadow-lg shadow-purple-900/20">
        <div className="max-w-4xl mx-auto flex justify-between items-center gap-4">
          <div className="flex items-center gap-3">
            <button
              onClick={() => setIsSidebarOpen(true)}
              className="p-2.5 rounded-xl bg-slate-800 hover:bg-slate-700 text-slate-300 border border-slate-700 transition-colors shadow-sm focus:outline-none focus:ring-2 focus:ring-slate-500"
              aria-label="Abrir menu de etapas"
            >
              <svg className="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path strokeLinecap="round" strokeLinejoin="round" strokeWidth="2.5" d="M4 6h16M4 12h16M4 18h16" />
              </svg>
            </button>

            <div>
              <h1 
                onClick={() => { setActiveView('creator'); setCurrentStep(-1); }} 
                className="text-2xl md:text-3xl font-bold bg-clip-text text-transparent bg-gradient-to-r from-purple-400 to-indigo-400 cursor-pointer"
              >
                Forja Astral
              </h1>
              <p className="text-slate-400 text-xs md:text-sm hidden sm:block">
                Criação de Servidores Mágickos baseada nos preceitos da Magia do Caos
              </p>
            </div>
          </div>
          
          <button
            onClick={() => setActiveView('grimoire')}
            className={`flex items-center gap-2 px-4 py-2 rounded-xl border transition-all shadow-sm ${
              activeView === 'grimoire' || activeView === 'view_contract'
                ? 'bg-purple-900/40 text-purple-200 border-purple-500'
                : 'bg-slate-800 hover:bg-slate-700 text-purple-300 border-purple-900/50'
            }`}
          >
            <svg className="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path strokeLinecap="round" strokeLinejoin="round" strokeWidth="2" d="M12 6.253v13m0-13C10.832 5.477 9.246 5 7.5 5S4.168 5.477 3 6.253v13C4.168 18.477 5.754 18 7.5 18s3.332.477 4.5 1.253m0-13C13.168 5.477 14.754 5 16.5 5c1.747 0 3.332.477 4.5 1.253v13C19.832 18.477 18.247 18 16.5 18c-1.746 0-3.332.477-4.5 1.253" /></svg>
            <span className="hidden sm:inline font-medium">Grimório</span>
          </button>
        </div>
      </header>

      {/* MAIN CONTAINER */}
      <main className="flex-1 max-w-4xl w-full mx-auto p-6 print:p-0 flex flex-col justify-center">
        
        {/* LANDING PAGE */}
        {activeView === 'creator' && currentStep === -1 && (
          <div className="py-8 md:py-12 flex flex-col items-center text-center">
            
            <div className="relative w-64 h-64 md:w-80 md:h-80 my-8 flex items-center justify-center">
              {/* Outer Rotating Ring */}
              <div className="absolute inset-0 rounded-full border-2 border-purple-500/30 animate-spin-ring"></div>
              <div className="absolute inset-4 rounded-full border border-dashed border-indigo-500/40"></div>
              <div className="absolute inset-10 rounded-full border border-purple-400/20"></div>

              {/* Static Center Emblem: SVG de Forja */}
              <div className="w-36 h-36 md:w-44 md:h-44 rounded-full bg-slate-950 shadow-[0_0_50px_rgba(168,85,247,0.4)] flex items-center justify-center border-2 border-purple-400/60 z-10 p-5 overflow-hidden text-purple-400">
                <svg viewBox="0 0 200 200" xmlns="http://www.w3.org/2000/svg" className="w-full h-full drop-shadow-[0_0_12px_rgba(192,132,252,0.6)]">
                  <defs>
                      <path id="star" d="M 0 -10 Q 0 0 10 0 Q 0 0 0 10 Q 0 0 -10 0 Q 0 0 0 -10 Z" fill="currentColor" />
                      <path id="spark" d="M 0,-12 L 2,0 L 0,12 L -2,0 Z" fill="currentColor" opacity="0.8" />
                      
                      <mask id="anvilMask">
                          <rect x="0" y="0" width="200" height="200" fill="white" />
                          <use href="#star" transform="translate(100, 128) scale(0.65)" fill="black" />
                          <path d="M 67 108 L 128 108" stroke="black" strokeWidth="2" />
                      </mask>

                      <mask id="hammerMask">
                          <rect x="-50" y="-100" width="100" height="150" fill="white" />
                          <line x1="-7" y1="-30" x2="7" y2="-30" stroke="black" strokeWidth="1.5" />
                          <line x1="-7" y1="-35" x2="7" y2="-35" stroke="black" strokeWidth="1.5" />
                          <line x1="-7" y1="-40" x2="7" y2="-40" stroke="black" strokeWidth="1.5" />
                          <rect x="-14" y="-8" width="28" height="16" rx="1" fill="none" stroke="black" strokeWidth="1.5" />
                      </mask>
                  </defs>

                  <circle cx="100" cy="100" r="85" fill="none" stroke="currentColor" strokeWidth="0.3" opacity="0.4" />

                  <use href="#star" transform="translate(100, 15) scale(0.8)" />
                  <use href="#star" transform="translate(100, 185) scale(0.8)" />
                  <use href="#star" transform="translate(15, 100) scale(0.8)" />
                  <use href="#star" transform="translate(185, 100) scale(0.8)" />

                  <use href="#star" transform="translate(50, 60) scale(0.4)" opacity="0.6" />
                  <use href="#star" transform="translate(150, 50) scale(0.5)" opacity="0.6" />
                  <use href="#star" transform="translate(160, 140) scale(0.3)" opacity="0.6" />
                  <use href="#star" transform="translate(40, 150) scale(0.4)" opacity="0.6" />

                  <use href="#spark" transform="translate(55, 95) rotate(-65)" />
                  <use href="#spark" transform="translate(70, 75) rotate(-35)" />
                  <use href="#spark" transform="translate(125, 80) rotate(40)" />
                  <use href="#spark" transform="translate(140, 105) rotate(75)" />

                  <g id="anvil" mask="url(#anvilMask)">
                      <path d="
                          M 70 150
                          L 130 150
                          L 125 140
                          L 115 140
                          Q 115 125 130 115
                          L 130 105
                          L 65 105
                          Q 45 105 25 115
                          Q 40 120 55 120
                          Q 75 125 80 140
                          L 75 140
                          Z" fill="currentColor" />
                  </g>

                  <g id="hammer" transform="translate(95, 100) rotate(35)" mask="url(#hammerMask)">
                      <rect x="-6" y="-65" width="12" height="65" rx="2" fill="currentColor" />
                      <rect x="-8" y="-70" width="16" height="8" rx="2" fill="currentColor" />
                      <rect x="-18" y="-12" width="36" height="24" rx="3" fill="currentColor" />
                  </g>
                </svg>
              </div>

              {/* Orbiting Symbols */}
              <div className="absolute inset-0 animate-orbit-symbols pointer-events-none">
                {orbitingSymbols.map((item, index) => {
                  const angle = (index / orbitingSymbols.length) * 2 * Math.PI;
                  const radius = 115;
                  const x = Math.cos(angle) * radius;
                  const y = Math.sin(angle) * radius;
                  
                  return (
                    <div
                      key={index}
                      className="absolute w-10 h-10 rounded-full bg-slate-900 border border-purple-500/40 text-purple-300 flex items-center justify-center text-lg shadow-lg shadow-purple-950 pointer-events-auto transition-transform hover:scale-125"
                      style={{
                        top: `calc(50% + ${y}px - 20px)`,
                        left: `calc(50% + ${x}px - 20px)`
                      }}
                      title={item.label}
                    >
                      <div className="animate-keep-upright">
                        {item.sym}
                      </div>
                    </div>
                  );
                })}
              </div>
            </div>

            <h2 className="text-3xl md:text-4xl font-extrabold bg-clip-text text-transparent bg-gradient-to-r from-purple-300 via-indigo-300 to-purple-400 mb-4">
              Desperte sua Criação Astral
            </h2>
            <p className="text-slate-400 max-w-xl text-base md:text-lg mb-8 leading-relaxed">
              Dê forma e consciência a uma forma-pensamento através da Magia do Caos. Defina propósitos, trace sigilos planetários sagrados e materialize o contrato mágico.
            </p>

            <div className="flex flex-col sm:flex-row gap-4 w-full max-w-md justify-center">
              <button
                onClick={() => { setActiveView('creator'); setCurrentStep(0); }}
                className="bg-gradient-to-r from-purple-600 to-indigo-600 hover:from-purple-500 hover:to-indigo-500 text-white font-bold py-4 px-8 rounded-xl shadow-lg shadow-purple-900/40 transition-all transform hover:-translate-y-0.5"
              >
                Iniciar Forja Agora
              </button>
            </div>
          </div>
        )}

        {/* STEPPER FLOW */}
        {activeView === 'creator' && currentStep >= 0 && currentStep < STEPS.length && (
          <div>
            <div className="print:hidden mb-6">
              <div className="flex justify-between items-center text-xs font-medium text-purple-400 mb-2">
                <span>Passo {currentStep + 1} de {STEPS.length}</span>
                <span>{STEPS[currentStep].title.split('.')[1]}</span>
              </div>
              <div className="h-2 w-full bg-slate-800 rounded-full overflow-hidden">
                <div 
                  className="h-full bg-gradient-to-r from-purple-500 to-indigo-500 transition-all duration-300"
                  style={{ width: `${((currentStep + 1) / STEPS.length) * 100}%` }}
                ></div>
              </div>
            </div>

            <div className="print:hidden bg-slate-900 border border-slate-800 rounded-2xl p-6 md:p-8 shadow-xl">
              <div className="space-y-6">
                <div>
                  <h2 className="text-2xl font-bold text-slate-100 mb-2">{STEPS[currentStep].title}</h2>
                  <p className="text-slate-400 leading-relaxed">{STEPS[currentStep].description}</p>
                </div>

                {STEPS[currentStep].type === 'text' && (
                  <input
                    type="text"
                    value={formData[STEPS[currentStep].id] || ''}
                    onChange={(e) => handleInputChange(STEPS[currentStep].id, e.target.value)}
                    placeholder={STEPS[currentStep].placeholder}
                    className="w-full bg-slate-950 border border-slate-700 text-slate-200 rounded-xl px-4 py-3 focus:outline-none focus:border-purple-500 focus:ring-1 focus:ring-purple-500 transition-all"
                  />
                )}

                {STEPS[currentStep].type === 'textarea' && (
                  <textarea
                    value={formData[STEPS[currentStep].id] || ''}
                    onChange={(e) => handleInputChange(STEPS[currentStep].id, e.target.value)}
                    placeholder={STEPS[currentStep].placeholder}
                    rows={4}
                    className="w-full bg-slate-950 border border-slate-700 text-slate-200 rounded-xl px-4 py-3 focus:outline-none focus:border-purple-500 focus:ring-1 focus:ring-purple-500 transition-all resize-none"
                  />
                )}

                {STEPS[currentStep].id === 'aparencia' && (
                  <div className="mt-6 pt-6 border-t border-slate-800 bg-slate-900/50 p-4 rounded-xl">
                    <h3 className="text-lg font-medium text-slate-200 mb-2 flex items-center gap-2">
                      <svg className="w-5 h-5 text-purple-400" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                        <path strokeLinecap="round" strokeLinejoin="round" strokeWidth="2" d="M19.428 15.428a2 2 0 00-1.022-.547l-2.387-.477a6 6 0 00-3.86.517l-.318.158a6 6 0 01-3.86.517L6.05 15.21a2 2 0 00-1.806.547M8 4h8l-1 1v5.172a2 2 0 00.586 1.414l5 5c1.26 1.26.367 3.414-1.415 3.414H4.828c-1.782 0-2.674-2.154-1.414-3.414l5-5A2 2 0 009 10.172V5L8 4z" />
                      </svg>
                      Materialização Visual (IA - Formato 9:16 Vertical)
                    </h3>
                    <p className="text-sm text-slate-400 mb-4">Gere automaticamente uma ilustração vertical baseada na sua descrição usando o Gemini.</p>
                    <div className="flex flex-wrap gap-3 mb-4">
                      <button
                        onClick={handleGenerateImage}
                        disabled={isGeneratingImage || !formData.aparencia.trim()}
                        className="bg-purple-600 hover:bg-purple-500 disabled:opacity-50 disabled:cursor-not-allowed text-white px-6 py-2.5 rounded-xl font-medium transition-colors shadow-lg shadow-purple-900/30"
                      >
                        {isGeneratingImage ? 'Canalizando...' : 'Gerar Imagem (9:16)'}
                      </button>

                      {generatedImage && (
                        <button
                          onClick={handleDownloadImagePDF}
                          className="bg-slate-800 hover:bg-slate-700 text-slate-200 px-5 py-2.5 rounded-xl font-medium transition-colors border border-slate-700 flex items-center gap-2"
                        >
                          <svg className="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path strokeLinecap="round" strokeLinejoin="round" strokeWidth="2" d="M4 16v1a3 3 0 003 3h10a3 3 0 003-3v-1m-4-4l-4 4m0 0l-4-4m4 4V4" /></svg>
                          Salvar Imagem em PDF
                        </button>
                      )}
                    </div>
                    
                    {generatedImage && (
                      <div className="mt-4 flex justify-center">
                        <img src={generatedImage} alt="Servidor Astral Gerado" className="w-36 h-64 object-cover rounded-xl shadow-lg border border-purple-900/50" />
                      </div>
                    )}
                  </div>
                )}

                {STEPS[currentStep].type === 'custom_sigil' && (
                  <div className="space-y-6 mt-4">
                    <div>
                      <label className="block text-sm text-slate-400 mb-2">Escolha o Planeta (Kamea)</label>
                      <div className="grid grid-cols-2 sm:grid-cols-3 md:grid-cols-4 gap-2.5">
                        {Object.entries(PLANETS).map(([key, p]) => (
                          <button
                            key={key}
                            onClick={() => setSigilPlanet(key)}
                            className={`p-3 rounded-xl border text-left transition-all flex flex-col gap-1 ${
                              sigilPlanet === key 
                                ? 'bg-purple-600/20 border-purple-500 text-purple-200 shadow-md shadow-purple-900/30 ring-1 ring-purple-500' 
                                : 'bg-slate-950/60 border-slate-800 text-slate-400 hover:bg-slate-800/50 hover:text-slate-200'
                            }`}
                          >
                            <div className="flex items-center justify-between">
                              <span className="font-semibold text-sm text-slate-100">{p.name}</span>
                              <span className="text-lg text-purple-400">{p.sym}</span>
                            </div>
                            <span className="text-[11px] text-slate-400 truncate capitalize">{p.keywords.split(',')[0]}</span>
                          </button>
                        ))}
                      </div>

                      {PLANETS[sigilPlanet] && (
                        <div className="mt-3 p-3.5 bg-slate-950/80 border border-slate-800 rounded-xl text-xs text-slate-300 leading-relaxed shadow-inner">
                          <span className="font-semibold text-purple-400 block mb-1">Atribuições de {PLANETS[sigilPlanet].name}:</span>
                          {PLANETS[sigilPlanet].description}
                        </div>
                      )}
                    </div>

                    <div>
                      <label className="block text-sm text-slate-400 mb-1">Intenção / Mantra do Sigilo</label>
                      <input 
                        type="text" 
                        value={sigilIntention} 
                        onChange={(e) => {
                          setSigilIntention(e.target.value);
                          setIsIntentionTouched(true);
                        }}
                        placeholder="Ex: PROPÓSITO DO SERVIDOR"
                        className="w-full bg-slate-950 border border-slate-700 text-slate-200 rounded-xl px-4 py-3 focus:outline-none focus:border-purple-500"
                      />
                    </div>

                    <div className="flex flex-wrap gap-4 items-center text-sm text-slate-300">
                      <label className="flex items-center gap-2 cursor-pointer">
                        <input type="checkbox" checked={sigilOptNumbers} onChange={(e) => setSigilOptNumbers(e.target.checked)} className="rounded text-purple-600 focus:ring-purple-500 bg-slate-900 border-slate-700" />
                        Mostrar números da Kamea
                      </label>
                    </div>

                    <div className="flex items-center gap-4">
                      <span className="text-sm text-slate-400 whitespace-nowrap">Suavidade do Traço:</span>
                      <input type="range" min="0" max="60" value={sigilSmooth} onChange={(e) => setSigilSmooth(e.target.value)} className="w-full accent-purple-500" />
                    </div>

                    <button 
                      onClick={handleGenerateSigil}
                      className="w-full bg-gradient-to-r from-purple-600 to-indigo-600 hover:from-purple-500 hover:to-indigo-500 text-white font-bold py-3 px-4 rounded-xl transition-all shadow-lg shadow-purple-900/30"
                    >
                      Gerar Sigilo Mágicko
                    </button>

                    {liveSigilSvg && (
                      <div className="mt-8 p-6 bg-slate-950 rounded-xl border border-slate-800 flex flex-col items-center gap-4">
                        <div className="w-full max-w-sm rounded-lg overflow-hidden shadow-2xl" dangerouslySetInnerHTML={{ __html: liveSigilSvg }} />
                        
                        <button 
                          onClick={handleConfirmSigil}
                          className="bg-green-600 hover:bg-green-500 text-white px-6 py-2 rounded-lg font-medium transition-colors"
                        >
                          Confirmar Este Sigilo
                        </button>
                      </div>
                    )}
                  </div>
                )}
              </div>

              <div className="mt-8 pt-6 border-t border-slate-800 flex justify-between items-center">
                <button
                  onClick={prevStep}
                  className="px-5 py-2.5 rounded-lg font-medium text-slate-300 hover:bg-slate-800 transition-colors"
                >
                  Voltar
                </button>
                
                <button
                  onClick={() => {
                    if(currentStep < STEPS.length - 1) nextStep();
                    else setCurrentStep(STEPS.length);
                  }}
                  className="bg-indigo-600 hover:bg-indigo-500 text-white px-6 py-2.5 rounded-lg font-medium transition-all shadow-lg shadow-indigo-900/30 flex items-center gap-2"
                >
                  {currentStep === STEPS.length - 1 ? 'Ver Contrato Final' : 'Próximo Passo'}
                  <svg className="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path strokeLinecap="round" strokeLinejoin="round" strokeWidth="2" d="M9 5l7 7-7 7" /></svg>
                </button>
              </div>
            </div>
          </div>
        )}

        {/* GRIMOIRE VIEW */}
        {activeView === 'grimoire' && (
          <div className="py-8 w-full">
            <div className="flex flex-col sm:flex-row justify-between items-start sm:items-center mb-8 gap-4">
              <div>
                <h2 className="text-3xl font-bold bg-clip-text text-transparent bg-gradient-to-r from-purple-400 to-indigo-400 font-cinzel">Seu Grimório</h2>
                <p className="text-slate-400 text-sm mt-1">O registro permanente das suas formas-pensamento.</p>
              </div>
              <button 
                onClick={() => { 
                  setFormData({ proposito: '', nome: '', aparencia: '', viaManifestacao: '', morte: '', chamado: '', alimentacao: '', comunicacao: '', ancora: '', palavraFatal: '', sigilo: '' });
                  setSigilIntention('');
                  setIsIntentionTouched(false);
                  setGeneratedImage(null);
                  setSigilMeta(null);
                  setTempSigilMeta(null);
                  setContractCustomText(null);
                  setActiveView('creator'); 
                  setCurrentStep(-1); 
                }} 
                className="bg-slate-800 hover:bg-slate-700 text-slate-200 px-5 py-2.5 rounded-xl text-sm font-medium transition-colors border border-slate-700 flex items-center gap-2"
              >
                <svg className="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path strokeLinecap="round" strokeLinejoin="round" strokeWidth="2" d="M12 4v16m8-8H4" /></svg>
                Novo Servidor
              </button>
            </div>

            {savedServitors.length === 0 ? (
              <div className="text-center py-20 bg-slate-900/50 rounded-2xl border border-slate-800 border-dashed">
                <svg className="w-16 h-16 text-slate-600 mx-auto mb-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path strokeLinecap="round" strokeLinejoin="round" strokeWidth="1.5" d="M12 6.253v13m0-13C10.832 5.477 9.246 5 7.5 5S4.168 5.477 3 6.253v13C4.168 18.477 5.754 18 7.5 18s3.332.477 4.5 1.253m0-13C13.168 5.477 14.754 5 16.5 5c1.747 0 3.332.477 4.5 1.253v13C19.832 18.477 18.247 18 16.5 18c-1.746 0-3.332.477-4.5 1.253" /></svg>
                <p className="text-slate-300 text-lg font-medium">Seu grimório está vazio.</p>
                <p className="text-slate-500 text-sm mt-2 max-w-sm mx-auto">Conclua a criação do seu primeiro servidor mágico e salve-o para que apareça aqui.</p>
              </div>
            ) : (
              <div className="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-6">
                {savedServitors.map((serv) => (
                  <div 
                    key={serv.id} 
                    onClick={() => { setViewingServitor(serv); setActiveView('view_contract'); }}
                    className="bg-slate-900 border border-slate-700 hover:border-purple-500 rounded-2xl overflow-hidden cursor-pointer transition-all hover:shadow-lg hover:shadow-purple-900/20 group flex flex-col"
                  >
                    <div className="h-44 bg-slate-950 relative flex items-center justify-center border-b border-slate-800 overflow-hidden">
                        {serv.generatedImage ? (
                            <img src={serv.generatedImage} alt={serv.formData.nome} className="w-full h-full object-cover opacity-70 group-hover:opacity-100 group-hover:scale-105 transition-all duration-500" />
                        ) : (
                            <span className="text-slate-700 font-cinzel text-sm uppercase tracking-widest">Sem Imagem</span>
                        )}
                        <div className="absolute inset-0 bg-gradient-to-t from-slate-900 to-transparent opacity-80"></div>
                        <h3 className="absolute bottom-3 left-4 right-4 text-xl font-bold text-white font-cinzel drop-shadow-md truncate">{serv.formData.nome || 'Sem Nome'}</h3>
                    </div>
                    <div className="p-5 flex-1 flex flex-col">
                      <div className="flex justify-between items-center mb-3">
                        <span className="text-[10px] uppercase tracking-wider text-slate-500 font-bold bg-slate-950 px-2 py-1 rounded">Registro Astral</span>
                        <span className="text-xs text-slate-500">{serv.date}</span>
                      </div>
                      <p className="text-sm text-slate-300 line-clamp-3 flex-1 leading-relaxed">
                        <span className="text-purple-400 font-semibold text-xs uppercase tracking-wider block mb-1">Propósito:</span>
                        {serv.formData.proposito || 'Nenhum propósito definido.'}
                      </p>
                    </div>
                  </div>
                ))}
              </div>
            )}
          </div>
        )}

        {/* UNIFIED FINAL CONTRACT SHEET (A4 PERGAMENE RESPONSIVE) */}
        {isShowingContract && (
          <div className="flex flex-col items-center py-4">
            
            {/* TOP ACTIONS BAR */}
            <div className="print:hidden w-full max-w-2xl flex justify-between items-center mb-6 px-1">
              <button 
                onClick={() => activeView === 'view_contract' ? setActiveView('grimoire') : prevStep()}
                className="flex items-center gap-1.5 text-indigo-400 hover:text-indigo-300 font-bold text-xs tracking-wider transition-colors uppercase bg-slate-900/90 px-3.5 py-2 rounded-xl border border-indigo-900/40 shadow-sm"
              >
                <svg className="w-4 h-4" fill="none" stroke="currentColor" strokeWidth="2.5" viewBox="0 0 24 24"><path strokeLinecap="round" strokeLinejoin="round" d="M10 19l-7-7m0 0l7-7m-7 7h18" /></svg>
                {activeView === 'view_contract' ? 'Voltar ao Grimório' : 'Voltar'}
              </button>

              <div className="flex items-center gap-3 text-xs font-bold tracking-wider uppercase">
                {activeView === 'creator' && (
                  <button 
                    onClick={() => setIsEditingText(!isEditingText)}
                    className={`px-3.5 py-2 rounded-xl transition-colors border shadow-sm ${
                      isEditingText 
                        ? 'bg-amber-600/20 text-amber-400 border-amber-600' 
                        : 'text-indigo-300 bg-slate-900/90 border-indigo-500/40 hover:border-indigo-400'
                    }`}
                  >
                    {isEditingText ? 'Concluir' : 'Editar Texto'}
                  </button>
                )}

                <div className="relative">
                  <button 
                    onClick={() => setShowSaveDropdown(!showSaveDropdown)}
                    className="px-3.5 py-2 rounded-xl transition-colors border text-indigo-300 bg-slate-900/90 border-indigo-500/40 hover:border-indigo-400 flex items-center gap-1.5 shadow-sm"
                  >
                    {activeView === 'view_contract' ? 'Exportar' : 'Salvar / Exportar'}
                    <svg className={`w-3.5 h-3.5 transition-transform ${showSaveDropdown ? 'rotate-180' : ''}`} fill="none" stroke="currentColor" strokeWidth="2.5" viewBox="0 0 24 24"><path strokeLinecap="round" strokeLinejoin="round" d="M19 9l-7 7-7-7" /></svg>
                  </button>

                  {showSaveDropdown && (
                    <div className="absolute right-0 mt-2 w-52 bg-slate-900 border border-indigo-900/50 rounded-xl shadow-2xl z-50 overflow-hidden py-1">
                      {activeView === 'creator' && (
                        <button onClick={handleSaveToGrimoire} className="w-full text-left px-4 py-3 text-xs font-bold text-purple-300 hover:bg-indigo-900/40 transition-colors flex items-center gap-2">
                          <svg className="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path strokeLinecap="round" strokeLinejoin="round" strokeWidth="2" d="M8 7H5a2 2 0 00-2 2v9a2 2 0 002 2h14a2 2 0 002-2V9a2 2 0 00-2-2h-3m-1 4l-3 3m0 0l-3-3m3 3V4" /></svg>
                          Salvar no Grimório
                        </button>
                      )}
                      <button onClick={handleSaveAction} className={`w-full text-left px-4 py-2.5 text-xs text-indigo-300 hover:bg-indigo-900/40 transition-colors ${activeView === 'creator' ? 'border-t border-slate-800' : ''}`}>Exportar PDF (A4)</button>
                    </div>
                  )}
                </div>
              </div>
            </div>

            {/* A4 PAPER SHEET CONTAINER (RESPONSIVO NA TELA) */}
            <div 
              id="contract-paper-content" 
              className="w-full max-w-2xl text-[#2c1d0c] p-8 md:p-12 rounded-sm relative flex flex-col justify-between box-border"
              style={{
                backgroundColor: '#f0e4c8',
                backgroundImage: `
                  radial-gradient(ellipse 60% 45% at 12% 8%, rgba(139,101,45,0.22) 0%, transparent 60%),
                  radial-gradient(ellipse 55% 40% at 88% 12%, rgba(120,85,40,0.18) 0%, transparent 55%),
                  radial-gradient(ellipse 65% 50% at 82% 92%, rgba(150,108,50,0.22) 0%, transparent 60%),
                  radial-gradient(ellipse 50% 45% at 8% 88%, rgba(130,92,42,0.2) 0%, transparent 55%),
                  radial-gradient(ellipse 40% 30% at 50% 45%, rgba(180,150,100,0.12) 0%, transparent 60%),
                  radial-gradient(circle at 50% 50%, transparent 35%, rgba(101,67,33,0.28) 100%)
                `,
                boxShadow: 'inset 0 0 70px rgba(90,60,25,0.4), inset 0 0 15px rgba(90,60,25,0.3), 0 25px 50px -12px rgba(0,0,0,0.5)'
              }}
            >
              <div>
                <div className="text-center pb-6 mb-8 border-b border-[#d2c4aa]/50">
                  <h2 className="text-3xl md:text-4xl font-bold tracking-widest uppercase mb-1 font-cinzel text-[#2c1d0c]">
                    Contrato Mágicko
                  </h2>
                  <p className="text-xs uppercase tracking-widest text-[#5c4a32]">Servidor Astral de Manifestação</p>
                </div>

                {isEditingText && activeView === 'creator' ? (
                  <textarea
                    value={contractCustomText !== null ? contractCustomText : `Eu, ________________________, pelo poder da minha Vontade e através da Magia do Caos, dou vida e propósito a este servidor.

Seu nome é ${contractData.nome || '         '}. Você é um servidor astral autônomo criado com o único propósito de: ${contractData.proposito || '                                           '}.

Sua manifestação e aparência etérea se configuram como: ${contractData.aparencia || '                                           '}.

Para agir no plano físico, sua via de manifestação oficial será: ${contractData.viaManifestacao || '                                           '}.

Sempre que realizar seus deveres ou trabalhar em prol da minha vontade, você se alimentará de: ${contractData.alimentacao || '                                           '}.

Você atenderá prontamente ao meu chamado sempre que eu proferir seu nome seguido da palavra de evocação: ${contractData.chamado || '         '}.

Sua existência está firmemente ligada ao mundo material através da âncora física: ${contractData.ancora || '                                           '}.

Nossa comunicação fluirá principalmente através de: ${contractData.comunicacao || '                                           '}.

Sua vida e essência subsistirão até que ${contractData.morte || '                                           '}. No momento em que eu proferir sua palavra fatal ${contractData.palavraFatal || '         '}, sua estrutura se dissolverá instantaneamente, retornando ao Caos primordial.`}
                    onChange={(e) => setContractCustomText(e.target.value)}
                    rows={16}
                    className="w-full bg-transparent border-none text-[#2c1d0c] font-cursive-contract text-2xl leading-relaxed focus:outline-none resize-y"
                  />
                ) : (
                  <div className="space-y-6 text-justify text-2xl leading-loose font-cursive-contract text-[#2c1d0c]">
                    {contractText != null ? (
                      <div dangerouslySetInnerHTML={{ __html: contractText.replace(/\n/g, '<br/>') }} />
                    ) : (
                      <>
                        <p>
                          Eu, ________________________, pelo poder da minha Vontade e através da Magia do Caos, dou vida e propósito a este servidor.
                        </p>
                        <p>
                          Seu nome é <span className="font-bold">{contractData.nome || '\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0'}</span>. Você é um servidor astral autônomo criado com o único propósito de: {contractData.proposito || '\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0'}.
                        </p>
                        <p>
                          Sua manifestação e aparência etérea se configuram como: {contractData.aparencia || '\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0'}.
                        </p>
                        <p>
                          Para agir no plano físico, sua via de manifestação oficial será: {contractData.viaManifestacao || '\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0'}.
                        </p>
                        <p>
                          Sempre que realizar seus deveres ou trabalhar em prol da minha vontade, você se alimentará de: {contractData.alimentacao || '\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0'}.
                        </p>
                        <p>
                          Você atenderá prontamente ao meu chamado sempre que eu proferir seu nome seguido da palavra de evocação: <span className="font-bold">{contractData.chamado || '\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0'}</span>.
                        </p>
                        <p>
                          Sua existência está firmemente ligada ao mundo material através da âncora física: {contractData.ancora || '\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0'}.
                        </p>
                        <p>
                          Nossa comunicação fluirá principalmente através de: {contractData.comunicacao || '\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0'}.
                        </p>
                        <p>
                          Sua vida e essência subsistirão até que {contractData.morte || '\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0'}. No momento em que eu proferir sua palavra fatal <span className="font-bold">{contractData.palavraFatal || '\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0\u00A0'}</span>, sua estrutura se dissolverá instantaneamente, retornando ao Caos primordial.
                        </p>
                      </>
                    )}
                  </div>
                )}
              </div>

              {/* SEÇÃO SIGILO COM O CÍRCULO & IMAGEM */}
              <div className="mt-12 pt-6 grid grid-cols-1 sm:grid-cols-2 gap-8 items-center text-center">
                <div>
                  <h4 className="text-xs font-bold uppercase tracking-widest mb-3 font-cinzel text-[#5c4a32]">Sigilo de Evocação</h4>
                  {contractSigilSvg ? (
                    <div className="w-40 h-40 mx-auto bg-transparent flex items-center justify-center" dangerouslySetInnerHTML={{ __html: contractSigilSvg }} />
                  ) : (
                    <div className="w-40 h-40 mx-auto flex items-center justify-center text-xs text-[#7c6a52] font-cursive-contract text-xl"></div>
                  )}
                </div>

                <div>
                  <h4 className="text-xs font-bold uppercase tracking-widest mb-3 font-cinzel text-[#5c4a32]">Aparência</h4>
                  {contractImage ? (
                    <img src={contractImage} alt="Servidor Astral" className="w-24 h-40 object-cover mx-auto rounded shadow-sm border border-[#d2c4aa]" />
                  ) : (
                    <div className="w-24 h-40 mx-auto flex items-center justify-center text-xs text-[#7c6a52] font-cursive-contract text-xl p-2"></div>
                  )}
                </div>
              </div>

              {/* ASSINATURA CLEAN */}
              <div className="mt-16 pt-6 flex justify-between text-sm font-cinzel text-[#5c4a32]">
                <div>
                  <p>Data: ____/____/________</p>
                </div>
                <div className="text-center">
                  <p>Assinatura do Magista</p>
                </div>
              </div>
            </div>
          </div>
        )}

      </main>
    </div>
  );
}
