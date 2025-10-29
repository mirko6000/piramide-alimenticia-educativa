# piramide-alimenticia-educativa
Piramide alimenticia
<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Pirámide Alimenticia Interactiva</title>
<style>
  :root{
    --bg:#e8f6ef;
    --accent:#00796b;
    --panel-bg:#ffffff;
    --card-shadow: 0 6px 18px rgba(0,0,0,0.12);
    --radius:14px;
  }
  html,body{height:100%;margin:0;}
  body{
    font-family: "Poppins", system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial;
    background:var(--bg);
    color:#1b1b1b;
    display:flex;
    flex-direction:column;
    align-items:center;
    padding:28px;
    -webkit-font-smoothing:antialiased;
  }

  header{width:100%;max-width:1200px;display:flex;justify-content:center;align-items:center;}
  h1{color:var(--accent);margin:6px 0 0 0;font-size:clamp(1.4rem,2.2vw,2rem);text-align:center}

  .main {
    width:100%;
    max-width:1200px;
    margin-top:22px;
    display:grid;
    grid-template-columns: 1fr 480px;
    gap:30px;
    align-items:start;
  }

  /* Left column: pirámide + instructions */
  .left {
    display:flex;
    flex-direction:column;
    gap:18px;
    align-items:center;
  }

  .instructions{
    background:var(--panel-bg);
    padding:14px 18px;
    border-radius:var(--radius);
    box-shadow:var(--card-shadow);
    width:100%;
  }
  .instructions p{margin:6px 0;font-size:0.95rem}
  .instructions small{color:#666}

  /* Pirámide container */
  .pyramid-wrap{
    width: min(680px,92%);
    display:flex;
    justify-content:center;
  }

  /* Single triangular pyramid with internal stripes */
  .pyramid {
    position:relative;
    width:520px;
    height:700px;
    max-width:92vw;
    clip-path: polygon(50% 0%, 0% 100%, 100% 100%);
    background:
      linear-gradient(
        to bottom,
        #ffdac2 0%,
        #ffdac2 16.666%,
        #ffeaa7 16.666%,
        #ffeaa7 33.333%,
        #fff6d1 33.333%,
        #fff6d1 50%,
        #d9f0c9 50%,
        #d9f0c9 66.666%,
        #bfe9b6 66.666%,
        #bfe9b6 83.333%,
        #9fd6a3 83.333%,
        #9fd6a3 100%
      );
    box-shadow:0 10px 30px rgba(0,0,0,0.12);
    display:block;
  }

  /* Overlay clickable bands positioned within the triangle */
  .band{
    position:absolute;
    left:0;
    width:100%;
    height:16.6666%;
    display:flex;
    align-items:center;
    justify-content:center;
    font-weight:600;
    color:#1b1b1b;
    text-align:center;
    cursor:pointer;
    transition:transform .18s ease, filter .18s;
    -webkit-tap-highlight-color: transparent;
    user-select:none;
    padding:6px;
    font-size:clamp(0.95rem,1.2vw,1.12rem);
  }
  .band:active{transform:scale(.995)}
  .band:hover{transform:scale(1.02);filter:brightness(1.02)}

  /* from top (smallest) to base (largest) */
  .band-6{top:0%}    /* Grasas, aceites y dulces - punta */
  .band-5{top:16.6666%}
  .band-4{top:33.3332%}
  .band-3{top:49.9998%}
  .band-2{top:66.6664%}
  .band-1{top:83.333%}  /* Granos - base */

  .band .label{
    display:flex;
    gap:8px;
    align-items:center;
    justify-content:center;
    padding:8px 14px;
    background:rgba(255,255,255,0.18);
    border-radius:999px;
    backdrop-filter: blur(4px);
  }

  /* Right column: panel + detail drawer */
  .right{
    position:relative;
    display:flex;
    flex-direction:column;
    gap:16px;
    align-items:stretch;
  }

  .panel {
    background:var(--panel-bg);
    padding:18px;
    border-radius:var(--radius);
    box-shadow:var(--card-shadow);
    width:100%;
    min-height:240px;
    max-height:560px;
    overflow:auto;
    font-size:clamp(0.82rem, 0.9vw, 1rem);
  }

  .panel h2{margin:0 0 8px 0;color:var(--accent);font-size:clamp(1rem,1.2vw,1.2rem)}
  .panel p{margin:8px 0 12px 0;color:#444}

  .list-group{margin:8px 0 0 0;padding:0;list-style:none}
  .list-group li{
    padding:8px 10px;border-radius:10px;margin:6px 0;cursor:pointer;
    transition:background .12s, transform .12s;
    display:flex;align-items:center;gap:10px;
  }
  .list-group li:hover{background:#f1fff6;transform:translateX(4px)}
  .list-group li .emoji{font-size:1.25rem}
  .list-group li .name{font-weight:600}

  /* Drawer (lateral) */
  .drawer {
    position:fixed;
    right:20px;
    top:60px;
    width:420px;
    max-width:92vw;
    height:calc(100vh - 120px);
    background:var(--panel-bg);
    border-radius:16px;
    box-shadow:0 20px 50px rgba(0,0,0,0.18);
    transform:translateX(110%);
    transition:transform .32s cubic-bezier(.2,.9,.2,1);
    z-index:1200;
    overflow:auto;
    padding:20px;
    display:flex;
    flex-direction:column;
    gap:12px;
  }
  .drawer.open{transform:translateX(0%)}

  .drawer .top{
    display:flex;gap:12px;align-items:center;justify-content:space-between;
  }
  .food-emoji{font-size:48px}
  .food-title{display:flex;flex-direction:column}
  .food-title h3{margin:0;font-size:1.25rem;color:#004d40}
  .food-title small{color:#2e7d62}

  .close-btn{
    background:transparent;border:none;color:#777;font-size:1.1rem;cursor:pointer;
  }

  .food-desc{margin-top:6px;color:#333;line-height:1.45}
  .food-list{margin-top:10px;padding:0;list-style:none}
  .food-list li{margin:8px 0;padding:6px 8px;border-radius:8px;background:#fbfff9}

  /* footer */
  footer{margin-top:18px;opacity:0.9;color:#555;font-size:0.9rem}

  /* responsive adjustments */
  @media (max-width: 980px){
    .main{grid-template-columns:1fr}
    .right{order:2}
    .left{order:1}
    .drawer{right:10px;top:80px;width:92vw;height:60vh}
  }
  @media (max-width:520px){
    .pyramid{width:92vw;height:62vh}
    .band{font-size:0.95rem}
    .drawer{top:64px;height:62vh}
  }
</style>
</head>
<body>

<header>
  <h1>🍽️ Pirámide Alimenticia Interactiva — completa</h1>
</header>

<div class="main" role="main">

  <!-- LEFT: Pyramid + small guide -->
  <div class="left">
    <div class="instructions">
      <p><strong>Cómo usar:</strong> Haz clic en cualquier franja de la pirámide para ver la categoría. En el panel, selecciona un alimento (lista con emojis). Cada alimento abrirá la ficha detallada a la derecha.</p>
      <small>Diseño educativo — responsivo — fichas detalladas por alimento.</small>
    </div>

    <div class="pyramid-wrap">
      <div class="pyramid" aria-hidden="true">
        <!-- bands top->base -->
        <div class="band band-6" data-cat="grasas" tabindex="0" onclick="showCategory('grasas')">
          <div class="label"><span>🍰 Grasas, Aceites y Dulces</span></div>
        </div>
        <div class="band band-5" data-cat="proteinas" tabindex="0" onclick="showCategory('proteinas')">
          <div class="label"><span>🥩 Proteínas</span></div>
        </div>
        <div class="band band-4" data-cat="lacteos" tabindex="0" onclick="showCategory('lacteos')">
          <div class="label"><span>🥛 Lácteos</span></div>
        </div>
        <div class="band band-3" data-cat="frutas" tabindex="0" onclick="showCategory('frutas')">
          <div class="label"><span>🍎 Frutas</span></div>
        </div>
        <div class="band band-2" data-cat="verduras" tabindex="0" onclick="showCategory('verduras')">
          <div class="label"><span>🥦 Verduras</span></div>
        </div>
        <div class="band band-1" data-cat="granos" tabindex="0" onclick="showCategory('granos')">
          <div class="label"><span>🌾 Granos y Cereales</span></div>
        </div>
      </div>
    </div>
  </div>

  <!-- RIGHT: Panel -->
  <div class="right">
    <div class="panel" id="panel">
      <h2>Haz clic en una categoría de la pirámide</h2>
      <p>Después elige un alimento para ver su ficha detallada.</p>
      <div id="listArea"></div>
    </div>
  </div>

</div>

<!-- Drawer: food details -->
<aside class="drawer" id="drawer" aria-hidden="true">
  <div class="top">
    <div style="display:flex;gap:12px;align-items:center">
      <div class="food-emoji" id="detailEmoji">🍎</div>
      <div class="food-title">
        <h3 id="detailName">Nombre del alimento</h3>
        <small id="detailGroup">Categoría</small>
      </div>
    </div>
    <div style="display:flex;gap:10px;align-items:center">
      <button class="close-btn" aria-label="Cerrar" onclick="closeDrawer()">❌</button>
    </div>
  </div>

  <div class="food-desc" id="detailDesc">Descripción larga y propiedades nutricionales.</div>

  <div>
    <strong>Principales nutrientes / beneficios:</strong>
    <ul class="food-list" id="detailProps"></ul>
  </div>

  <div style="margin-top:auto;font-size:0.9rem;color:#666">
    <em>Consejo:</em> las porciones y la frecuencia dependen de la edad, actividad y estado de salud.
  </div>
</aside>

<footer>
  <small>El contenido es informativo y educativo; no sustituye el consejo médico o nutricional profesional.</small>
</footer>

<script>
/* ---------- Data: categories, subgroups and foods (with detailed text) ---------- */
/* I included the foods you provided earlier and wrote detailed descriptions for each.
   Descriptions are educational (nutrientes, beneficios, recomendaciones). */

const DATA = {
  granos: {
    groupEmoji: "🌾",
    title: "Granos y Cereales",
    subgroups: [
      {
        name: "🍞 Cereales y granos integrales",
        items: [
          { id:"arroz", label:"Arroz 🍚 (integral)", emoji:"🍚",
            desc: "El arroz integral conserva el salvado y el germen, por lo que aporta fibra, vitaminas del grupo B y minerales. Es una fuente de energía sostenida y mejora la salud intestinal. Comparado con el arroz blanco, tiene índice glucémico más bajo y ayuda en el control de la glucosa." ,
            props:["Fibra dietética: favorece tránsito intestinal","Vitaminas B: metabolismo energético","Minerales: magnesio y fósforo"] 
          },
          { id:"avena", label:"Avena 🌾", emoji:"🌾",
            desc: "La avena es rica en fibra soluble (betaglucanos) que ayuda a reducir el colesterol LDL. Aporta carbohidratos complejos de liberación lenta, proteínas vegetales y micronutrientes como hierro y magnesio. Es excelente en desayunos para mantener saciedad y control glucémico.",
            props:["Betaglucanos: reducen colesterol","Fibra soluble: saciante","Buena fuente de minerales"]
          },
          { id:"quinua", label:"Quinua", emoji:"🍽️",
            desc: "La quinua es un pseudocereal de alto valor proteico y perfil de aminoácidos cercano al de las proteínas animales. Aporta carbohidratos complejos, fibra y minerales como hierro y magnesio. Es apta para dietas sin gluten y útil para incrementar densidad nutricional en comidas." ,
            props:["Proteínas completas","Libre de gluten","Hierro y magnesio"]
          },
          { id:"trigo", label:"Trigo", emoji:"🌾",
            desc: "El trigo integral aporta energía, fibra y vitaminas del grupo B. Sus derivados integrales (pan, harinas integrales) conservan nutrientes que favorecen la salud digestiva. En personas sensibles al gluten conviene moderar su consumo o elegir alternativas sin gluten.",
            props:["Carbohidratos complejos","Fibra (integral)","Vitaminas del complejo B"]
          },
          { id:"cebad", label:"Cebada", emoji:"🌾",
            desc: "La cebada es un cereal rico en fibra soluble e insoluble, con beneficios para la salud digestiva y la reducción del colesterol. Se utiliza en sopas y guisos y tiene un perfil nutricional que incluye vitaminas y minerales. Es recomendable consumirla integral." ,
            props:["Fibra soluble: controla lípidos","Fuente de minerales","Baja IG en integral"]
          },
          { id:"maiz", label:"Maíz 🌽", emoji:"🌽",
            desc: "El maíz aporta carbohidratos, fibra y antioxidantes como los carotenoides. Las versiones integrales (granos enteros, mazorca) conservan más nutrientes que los productos muy procesados. Es fuente de energía y puede formar parte de una dieta equilibrada." ,
            props:["Carbohidratos y fibra","Antioxidantes carotenoides","Energético"]
          },
          { id:"amaranto", label:"Amaranto", emoji:"🌾",
            desc: "El amaranto es otro pseudocereal con buena cantidad de proteínas y lisina, un aminoácido limitante en otros granos. Aporta hierro, calcio y otros micronutrientes. Es útil para variar fuentes de carbohidratos y aumentar la calidad proteica." ,
            props:["Proteínas de alta calidad","Rico en minerales","Apto para combinaciones nutricionales"]
          },
          { id:"centeno", label:"Centeno", emoji:"🌾",
            desc: "El centeno integral contiene fibra y compuestos bioactivos que favorecen la salud intestinal y pueden reducir el riesgo de enfermedades metabólicas. Es habitual en panes integrales y tiene IG más bajo que harinas refinadas." ,
            props:["Fibra abundante","Bajo IG relativo","Usado en panes integrales"]
          }
        ]
      },
      {
        name: "🥯 Derivados de cereales",
        items: [
          { id:"pan", label:"Pan 🍞", emoji:"🍞",
            desc: "El pan integral aporta carbohidratos complejos, fibra y ciertas vitaminas si está fortificado. Es una fuente práctica de energía; elige versiones integrales para mayor aporte de fibra y mejor control glucémico. El consumo debe ajustarse a la porción en función de necesidades energéticas.",
            props:["Carbohidratos energéticos","Mejor integral por fibra","Evitar exceso de refinados"]
          },
          { id:"pasta", label:"Fideos / Pasta 🍝", emoji:"🍝",
            desc: "La pasta de grano entero aporta energía sostenida y más fibra que la refinada. Cocinada al dente tiene menor índice glucémico. Es una buena base para platos equilibrados si se acompaña con verduras y proteínas magras.",
            props:["Carbohidratos complejos","Mejor integral por fibra","Versátil en comidas balanceadas"]
          },
          { id:"galletas", label:"Galletas integrales", emoji:"🍪",
            desc: "Las galletas integrales pueden ofrecer algo de fibra, pero suelen contener azúcares y grasas añadidas; conviene leer etiquetas y priorizar versiones con ingredientes simples y sin exceso de azúcares. Útiles como snack ocasional más que diario.",
            props:["Fuente de carbohidratos","Revisar azúcares añadidos","Consumir con moderación"]
          },
          { id:"tortilla", label:"Tortillas de maíz o trigo 🌮", emoji:"🌮",
            desc: "Las tortillas de maíz tradicionales son una buena fuente de carbohidratos y, si son de maíz nixtamalizado, aportan más calcio y niacina. Las de trigo integral aportan más fibra. Son prácticas para preparar raciones controladas." ,
            props:["Carbohidratos prácticos","Seleccione integral o nixtamalizado","Buena base para comidas"]
          },
          { id:"cereal-hojuelas", label:"Cereal en hojuelas", emoji:"🥣",
            desc: "Los cereales en hojuelas integrales (sin azúcares añadidos) son una opción rápida de desayuno que aporta fibra y energía. Elegir versiones sin azúcares o con alto contenido de fibra mejora su calidad nutricional.",
            props:["Rápido desayuno","Mejor integral/sin azúcares","Combinar con fruta y leche"]
          }
        ]
      }
    ]
  },

  verduras: {
    groupEmoji: "🥦",
    title: "Verduras y Hortalizas",
    subgroups:[
      {
        name: "Verduras verdes",
        items:[
          { id:"lechuga", label:"Lechuga 🥬", emoji:"🥬",
            desc: "La lechuga aporta agua, fibra y pequeñas cantidades de vitaminas y minerales. Es excelente para aumentar el volumen del plato con pocas calorías y mejorar la hidratación. Combina bien en ensaladas y aporta sensación de saciedad.",
            props:["Baja en calorías","Hidratante","Fuente de fibra"]
          },
          { id:"espinaca", label:"Espinaca 🥬", emoji:"🥬",
            desc: "La espinaca contiene hierro no hemo, folato, vitamina K y antioxidantes como la luteína. Favorece la salud ósea (vitamina K) y la función celular. Cocinarla ligeramente mejora la absorción de algunos nutrientes.",
            props:["Hierro y folatos","Vitamina K para huesos","Antioxidantes"]
          },
          { id:"brocoli", label:"Brócoli 🥦", emoji:"🥦",
            desc: "El brócoli es rico en vitamina C, fibra, folato y compuestos bioactivos (isotiocianatos) con propiedades protectoras. Favorece la salud celular y aporta fibra para la microbiota. Puede consumirse al vapor para conservar nutrientes.",
            props:["Vitamina C alta","Compuestos fitoquímicos","Fibra prebiótica"]
          },
          { id:"acelga", label:"Acelga 🥬", emoji:"🥬",
            desc: "La acelga es fuente de vitaminas A y K, además de minerales como el magnesio. Es versátil en salteados y guisos; su aporte de fibra contribuye al tránsito intestinal y a la saciedad.",
            props:["Vitaminas A y K","Minerales","Fibra"]
          }
        ]
      },
      {
        name:"Otras verduras",
        items:[
          { id:"zanahoria", label:"Zanahoria 🥕", emoji:"🥕",
            desc: "La zanahoria es rica en betacaroteno, precursor de la vitamina A, clave para la salud visual y la piel. También aporta fibra y antioxidantes. Cruda o cocida conserva beneficios; combinada con grasas saludables mejora la absorción de carotenos.",
            props:["Betacaroteno: vitamina A","Fibra","Antioxidantes"]
          },
          { id:"tomate", label:"Tomate 🍅", emoji:"🍅",
            desc: "El tomate es rico en licopeno, un antioxidante asociado a beneficios cardiovasculares, además de aportar vitamina C y potasio. Consumirlo cocido puede aumentar la biodisponibilidad del licopeno.",
            props:["Licopeno antioxidante","Vitamina C","Potasio"]
          },
          { id:"cebolla", label:"Cebolla 🧅", emoji:"🧅",
            desc: "La cebolla aporta compuestos sulfurados y flavonoides con actividad antioxidante y antiinflamatoria. Suele usarse como base en muchas preparaciones y puede contribuir a la salud digestiva.",
            props:["Compuestos sulfurados","Antiinflamatorio leve","Saborizante natural"]
          },
          { id:"pimiento", label:"Pimiento 🫑", emoji:"🫑",
            desc: "El pimiento contiene vitamina C en cantidades elevadas, además de fibra y carotenoides. Es versátil en crudo o cocido y aporta color y nutrientes a las comidas.",
            props:["Vitamina C alta","Carotenoides","Fibra"]
          },
          { id:"coliflor", label:"Coliflor 🥦", emoji:"🥦",
            desc: "La coliflor es rica en fibra, vitamina C y compuestos con potencial antioxidante. Puede usarse como sustituto de carbohidratos en recetas bajas en calorías y es fuente de compuestos bioactivos.",
            props:["Fibra y vitamina C","Baja en calorías","Versátil culinariamente"]
          }
        ]
      }
    ]
  },

  frutas: {
    groupEmoji: "🍎",
    title: "Frutas",
    subgroups:[
      {
        name:"Cítricas (vitamina C)",
        items:[
          { id:"naranja", label:"Naranja 🍊", emoji:"🍊",
            desc: "La naranja es una fuente clásica de vitamina C, esencial para la función inmune y la síntesis de colágeno. Aporta fibra soluble (pectina) que ayuda al tránsito intestinal y al control del colesterol. Ideal como snack o en jugos naturales con moderación.",
            props:["Vitamina C alta","Fibra pectina","Antioxidantes"]
          },
          { id:"mandarina", label:"Mandarina 🍊", emoji:"🍊",
            desc: "La mandarina aporta vitamina C, flavonoides y azúcares naturales que ofrecen energía rápida. Es fácil de llevar como snack y contiene fibra para la digestión. Su peel tiene compuestos aromáticos con beneficios leves antioxidantes.",
            props:["Vitamina C","Flavonoides","Fibra"]
          },
          { id:"limon", label:"Limón 🍋", emoji:"🍋",
            desc: "El limón es rico en ácido cítrico y vitamina C. Se usa frecuentemente para mejorar la palatabilidad de platos y bebidas; su consumo puede facilitar la absorción de hierro no hemo cuando se combina con alimentos vegetales.",
            props:["Vitamina C","Mejora absorción de hierro","Ácido cítrico"]
          },
          { id:"pomelo", label:"Pomelo / toronja 🍈", emoji:"🍈",
            desc: "El pomelo aporta vitamina C, fibra y compuestos bioactivos. Interactúa con ciertos medicamentos; si estás bajo tratamiento, consulta con tu médico antes de consumir grandes cantidades. Nutricionalmente es refrescante y bajo en calorías.",
            props:["Vitamina C","Fibra","Precaución con medicación"]
          }
        ]
      },
      {
        name:"Tropicales",
        items:[
          { id:"platano", label:"Plátano 🍌", emoji:"🍌",
            desc: "El plátano es fuente de potasio, carbohidratos de rápida y media liberación y vitamina B6. Es ideal para reponer energía en actividad física y contribuir al equilibrio electrolítico. Su fibra ayuda al tránsito intestinal; elegir el punto de madurez según la tolerancia a los azúcares.",
            props:["Potasio alto","Energía rápida","Vitamina B6"]
          },
          { id:"pina", label:"Piña 🍍", emoji:"🍍",
            desc: "La piña aporta vitamina C, bromelina (enzima con efecto antiinflamatorio) y agua. Su consumo favorece la digestión de proteínas (por la bromelina) y aporta fibra. Se recomienda consumir fresca para maximizar nutrientes.",
            props:["Bromelina digestiva","Vitamina C","Fibra"]
          },
          { id:"mango", label:"Mango 🥭", emoji:"🥭",
            desc: "El mango es una fruta rica en vitamina A (carotenos), vitamina C y fibra. Es energizante y aporta antioxidantes que apoyan la salud ocular y la inmunidad. Consumir en porciones moderadas por su contenido de azúcares naturales.",
            props:["Carotenos y vitamina A","Vitamina C","Fibra"]
          },
          { id:"papaya", label:"Papaya", emoji:"🥭",
            desc: "La papaya aporta vitamina C, folato y la enzima papaína que favorece la digestión. Su pulpa es hidratante y nutritiva, útil para mejorar la digestión y como alimento suave para estómagos sensibles.",
            props:["Papaína digestiva","Vitamina C","Fibra suave"]
          },
          { id:"coco", label:"Coco 🥥", emoji:"🥥",
            desc: "El coco aporta grasas saturadas en su pulpa y aceite, además de minerales como potasio. Las versiones de leche o agua de coco pueden ser hidratantes; consumir con moderación por su contenido graso calórico.",
            props:["Grasas y minerales","Hidratante (agua)","Consumo moderado"]
          }
        ]
      },
      {
        name:"Del bosque y pequeñas",
        items:[
          { id:"fresa", label:"Fresas 🍓", emoji:"🍓",
            desc: "Las fresas contienen vitamina C, compuestos antioxidantes y fibra. Son bajas en calorías y pueden incorporarse a desayunos y postres saludables. Su consumo habitual aporta fitoquímicos beneficiosos para la salud cardiovascular.",
            props:["Vitamina C","Antioxidantes","Bajas calorías"]
          },
          { id:"arandanos", label:"Arándanos 🫐", emoji:"🫐",
            desc: "Los arándanos son ricos en antocianinas, pigmentos con potente actividad antioxidante y efectos positivos sobre la función cognitiva y vascular. Se recomiendan como parte de una dieta rica en frutas variadas.",
            props:["Antocianinas","Mejoran salud vascular","Antioxidantes"]
          },
          { id:"cerezas", label:"Cerezas 🍒", emoji:"🍒",
            desc: "Las cerezas aportan antioxidantes y compuestos antiinflamatorios, además de vitaminas y fibra. Pueden ayudar a la recuperación muscular tras ejercicio y mejorar la calidad del sueño en algunos casos.",
            props:["Antioxidantes y antiinflamatorios","Fibra","Beneficios para recuperación"]
          }
        ]
      },
      {
        name:"Templadas y jugosas",
        items:[
          { id:"manzana", label:"Manzana 🍎", emoji:"🍎",
            desc: "La manzana es una fruta rica en fibra soluble (pectina) y polifenoles que favorecen la microbiota intestinal y la salud cardiovascular. Es un snack práctico que aporta energía y saciedad. Consumir con piel para maximizar fibra y antioxidantes.",
            props:["Pectina y fibra","Polifenoles beneficiosos","Buen snack"]
          },
          { id:"pera", label:"Pera 🍐", emoji:"🍐",
            desc: "La pera aporta fibra soluble e insoluble, vitaminas y agua. Es suave para el sistema digestivo y útil para mantener la hidratación y el tránsito intestinal. Aporta antioxidantes y micronutrientes.",
            props:["Fibra abundante","Hidratante","Suave digestivamente"]
          },
          { id:"sandia", label:"Sandía 🍉", emoji:"🍉",
            desc: "La sandía es muy hidratante, rica en agua y contiene licopeno, un antioxidante. Es baja en calorías y refrescante, ideal en climas cálidos. Aporta algunas vitaminas y electrolitos, como potasio.",
            props:["Alta en agua","Licopeno antioxidante","Baja en calorías"]
          }
        ]
      }
    ]
  },

  lacteos: {
    groupEmoji:"🥛",
    title:"Lácteos y derivados",
    subgroups:[
      {
        name:"Leches",
        items:[
          { id:"leche_entera", label:"Leche entera 🥛", emoji:"🥛",
            desc: "La leche entera aporta proteínas de alta calidad, calcio, vitamina D (si está fortificada) y grasas que facilitan la absorción de vitaminas liposolubles. Es útil para crecimiento y mantenimiento óseo; elegir tipo según necesidades energéticas.",
            props:["Proteínas completas","Calcio para huesos","Vitamina D (si fortificada)"]
          },
          { id:"leche_descremada", label:"Leche descremada", emoji:"🥛",
            desc: "La leche descremada conserva proteínas y calcio pero con menor contenido graso. Es útil para reducir calorías sin perder nutrientes clave, aunque puede afectar la sensación de saciedad comparada con la entera.",
            props:["Proteínas","Calcio","Baja en grasa"]
          },
          { id:"leche_veg", label:"Leche vegetal fortificada 🥥", emoji:"🥥",
            desc: "Leches vegetales (soya, almendra, avena) fortificadas ofrecen calcio y vitamina D; su perfil de macronutrientes varía según la base. La leche de soya aporta proteínas vegetales; elegir versiones sin azúcares añadidos.",
            props:["Alternativa sin lactosa","Fortificada con calcio y D","Varía según base"]
          }
        ]
      },
      {
        name:"Quesos",
        items:[
          { id:"queso_fresco", label:"Queso fresco 🧀", emoji:"🧀",
            desc: "Los quesos frescos aportan calcio y proteínas; suelen ser menos grasos que los curados. Son una buena opción para añadir calcio y sabor, pero deben consumirse con moderación según su contenido graso y sodio.",
            props:["Calcio y proteína","Menos grasos (frescos)","Sodio a considerar"]
          },
          { id:"queso_mozz", label:"Mozzarella", emoji:"🧀",
            desc: "La mozzarella es relativamente baja en grasa comparada con otros quesos curados y aporta proteínas y calcio. Es apreciada en preparaciones calientes por su textura fundente.",
            props:["Proteínas","Calcio","Textura fundente para cocinar"]
          },
          { id:"queso_parm", label:"Parmesano", emoji:"🧀",
            desc: "El parmesano es un queso curado con alta densidad nutricional (calcio, proteínas) y mucho sabor; pequeñas porciones aportan sabor intenso. Debe consumirse en porciones moderadas por su contenido de sodio y grasa.",
            props:["Alta densidad de calcio","Sabor intenso","Porciones pequeñas"]
          }
        ]
      },
      {
        name:"Derivados",
        items:[
          { id:"yogur", label:"Yogur natural 🍶", emoji:"🍶",
            desc: "El yogur natural contiene proteínas de buena calidad y probióticos que favorecen la salud intestinal. Es una opción nutritiva para desayunos o snacks; preferir versiones sin azúcares añadidos para mantener beneficios.",
            props:["Probióticos: microbiota","Proteínas","Buen snack"]
          },
          { id:"mantequilla", label:"Mantequilla 🧈", emoji:"🧈",
            desc: "La mantequilla es una grasa láctea rica en grasas saturadas; aporta sabor pero debe consumirse con moderación. Elegir cantidades reducidas y preferir grasas insaturadas para el perfil cardiosaludable.",
            props:["Rica en grasas saturadas","Sabor intenso","Consumir con moderación"]
          },
          { id:"helado", label:"Helado 🍦", emoji:"🍦",
            desc: "El helado es un derivado lácteo que suele contener azúcares y grasas; puede disfrutarse ocasionalmente. Optar por porciones controladas y versiones con menor azúcar cuando sea posible.",
            props:["Alto en azúcares","Fuente de placer ocasional","Consumir con moderación"]
          }
        ]
      }
    ]
  },

  proteinas: {
    groupEmoji:"🥩",
    title:"Proteínas (animales y vegetales)",
    subgroups:[
      {
        name:"Carnes y pescados",
        items:[
          { id:"pollo", label:"Pollo 🍗", emoji:"🍗",
            desc: "El pollo es una carne magra rica en proteínas de alta calidad y con menor contenido graso en sus cortes magros. Aporta aminoácidos esenciales para la reparación muscular y el mantenimiento. Es versátil y recomendable como fuente proteica diaria si se eligen preparaciones saludables.",
            props:["Proteína magra","Aminoácidos esenciales","Bajo en grasa (pechuga)"]
          },
          { id:"pavo", label:"Pavo 🦃", emoji:"🦃",
            desc: "El pavo aporta proteínas magras y micronutrientes como selenio y vitaminas del grupo B. Es una opción adecuada en dietas bajas en grasa y para incrementar la ingesta proteica sin exceso calórico.",
            props:["Proteína magra","Selenio y B-vits","Baja grasa (dependiendo del corte)"]
          },
          { id:"res", label:"Carne de res 🥩", emoji:"🥩",
            desc: "La carne de res aporta proteínas completas, hierro hemo y vitamina B12, esenciales para el transporte de oxígeno y la salud neurológica. Consumir cortes magros y moderar la frecuencia; preferir métodos de cocción saludables.",
            props:["Hierro hemo","Vitamina B12","Proteínas completas"]
          },
          { id:"cerdo", label:"Cerdo 🍖", emoji:"🍖",
            desc: "El cerdo puede ser una fuente de proteína y ciertas vitaminas del grupo B; elegir cortes magros y técnicas de cocción que reduzcan grasa. Moderar el consumo de productos procesados (embutidos) por su contenido de sodio y grasas.",
            props:["Proteína","Vitaminas B","Preferir cortes magros"]
          },
          { id:"pescado", label:"Pescado 🐟", emoji:"🐟",
            desc: "El pescado (especialmente los grasos como salmón, sardina o atún) aporta proteínas de alta calidad y ácidos grasos omega-3 (DHA y EPA) beneficiosos para el corazón y el cerebro. Incluir pescado en la dieta 2–3 veces por semana es recomendable.",
            props:["Omega-3 (grasos)","Proteínas de alta calidad","Vitamina D en algunas especies"]
          },
          { id:"mariscos", label:"Mariscos 🦐", emoji:"🦐",
            desc: "Los mariscos (langostinos, calamar, pulpo) aportan proteínas magras, minerales como zinc y yodo y, en general, son bajos en grasa. Útiles para variar fuentes marinas; tener en cuenta posibles alergias.",
            props:["Proteínas magras","Minerales: zinc y yodo","Bajo en grasa"]
          }
        ]
      },
      {
        name:"Huevos y derivados",
        items:[
          { id:"huevo", label:"Huevo 🥚", emoji:"🥚",
            desc: "El huevo es una fuente completa de proteínas (incluye todos los aminoácidos esenciales), vitaminas (como B12, riboflavina) y minerales. La yema contiene colesterol, pero en personas sanas el consumo moderado es compatible con una dieta saludable; es muy útil para la reparación y síntesis muscular.",
            props:["Proteínas completas","Vitaminas B","Colesterol en yema (consumo moderado)"]
          },
          { id:"clara", label:"Clara de huevo", emoji:"🥚",
            desc: "La clara de huevo es prácticamente proteína pura, baja en calorías y sin grasas. Es utilizada frecuentemente para aumentar aporte proteico sin añadir grasa ni colesterol, ideal en planes de entrenamiento o recuperación.",
            props:["Proteína pura","Baja en calorías","Sin grasas ni colesterol"]
          }
        ]
      },
      {
        name:"Legumbres y frutos secos",
        items:[
          { id:"lentejas", label:"Lentejas 🟤", emoji:"🟤",
            desc: "Las lentejas son legumbres ricas en proteínas vegetales, fibra, hierro no hemo y folatos. Contribuyen al control glucémico y a la sensación de saciedad; combinadas con cereales aumentan la calidad proteica. Son económicas y nutritivas.",
            props:["Proteínas vegetales","Fibra y hierro","Buena alternativa a proteína animal"]
          },
          { id:"frejoles", label:"Frejoles / Porotos 🫘", emoji:"🫘",
            desc: "Los frejoles aportan proteínas, fibra, vitaminas del complejo B y minerales como el hierro. Favorecen la salud intestinal y el control de glucosa; su consumo regular aporta diversidad proteica y saciedad.",
            props:["Fibra abundante","Proteína vegetal","Micronutrientes B-vits"]
          },
          { id:"garbanzos", label:"Garbanzos", emoji:"🍛",
            desc: "Los garbanzos proporcionan proteínas vegetales, carbohidratos complejos y fibra. Son una buena fuente de energía sostenida y favorecen la digestión. Pueden formar parte de ensaladas, guisos y hummus.",
            props:["Proteína y carbohidratos complejos","Fibra","Versátiles en recetas"]
          },
          { id:"tofu", label:"Tofu (soya) 🍢", emoji:"🍢",
            desc: "El tofu es un derivado de soya con buena calidad proteica y textura versátil. Aporta proteína vegetal, hierro y calcio (según fortificación) y es útil en dietas vegetarianas o para reducir consumo de carne.",
            props:["Proteína vegetal","Versátil culinariamente","Fuente de minerales"]
          },
          { id:"maní", label:"Maní / Frutos secos 🥜", emoji:"🥜",
            desc: "Los frutos secos (maní, almendras, nueces, pistachos) contienen grasas insaturadas saludables, proteínas, fibra y micronutrientes. Aunque densos en energía, se recomiendan en porciones controladas por sus beneficios cardiovasculares.",
            props:["Grasas insaturadas","Fibra y proteína","Alto valor energético (porciones)"]
          }
        ]
      }
    ]
  },

  grasas: {
    groupEmoji:"🧈",
    title:"Grasas, Aceites y Dulces",
    subgroups:[
      {
        name:"Aceites y grasas saludables",
        items:[
          { id:"aceite_oliva", label:"Aceite de oliva 🫒", emoji:"🫒",
            desc: "El aceite de oliva extra virgen es una grasa insaturada con evidencia de beneficios cardiovasculares. Aporta ácidos grasos monoinsaturados y compuestos fenólicos con actividad antioxidante. Usarlo en crudo o moderadamente en cocción es recomendado.",
            props:["Monoinsaturados: cardioprotector","Compuestos fenólicos","Usar con moderación"]
          },
          { id:"aguacate", label:"Aguacate 🥑", emoji:"🥑",
            desc: "El aguacate aporta grasas monoinsaturadas, fibra, vitamina E y potasio. Contribuye a la saciedad y mejora la absorción de vitaminas liposolubles. Es calórico, por lo que conviene moderar la porción.",
            props:["Grasas saludables","Vitamina E","Fibra y potasio"]
          },
          { id:"frutos_secos", label:"Frutos secos 🥜", emoji:"🥜",
            desc: "Los frutos secos contienen grasas saludables, fibra, proteínas y micronutrientes (magnesio, vitamina E). Su consumo habitual en porciones puede reducir riesgo cardiovascular, pero hay que controlarlos por densidad energética.",
            props:["Grasas insaturadas","Micronutrientes","Porciones controladas"]
          },
          { id:"pescado_graso", label:"Pescados grasos 🐟", emoji:"🐟",
            desc: "Los pescados grasos (salmón, sardina, atún) son ricos en ácidos grasos omega-3 (DHA/EPA), asociados a beneficios cardiovasculares y neurológicos. Incluir pescado graso 1–3 veces por semana contribuye a una dieta equilibrada.",
            props:["Omega-3 DHA/EPA","Proteína de calidad","Salud cardiovascular"]
          }
        ]
      },
      {
        name:"Saturadas y dulces (limitar)",
        items:[
          { id:"mantequilla2", label:"Mantequilla 🧈", emoji:"🧈",
            desc: "La mantequilla contiene grasas saturadas y calorías concentradas; aporta sabor pero su consumo debe limitarse en favor de fuentes de grasa insaturada. Usar porciones pequeñas y evitar su uso excesivo.",
            props:["Alta en grasas saturadas","Sabor intenso","Usar con moderación"]
          },
          { id:"chocolate", label:"Chocolate 🍫", emoji:"🍫",
            desc: "El chocolate, especialmente el negro con alto porcentaje de cacao, aporta antioxidantes (flavonoides). Sin embargo, muchos productos comerciales contienen azúcares y grasas añadidas; preferir versiones con mayor cacao y porciones moderadas.",
            props:["Flavonoides (en cacao alto)","Azúcares en versiones comerciales","Consumir con moderación"]
          },
          { id:"helado2", label:"Helados y dulces 🍦", emoji:"🍦",
            desc: "Helados y postres azucarados aportan muchas calorías vacías (azúcar y grasas). Pueden disfrutarse ocasionalmente; para consumo frecuente, elegir opciones con menos azúcares añadidos y porciones controladas.",
            props:["Alto en azúcar y grasa","Calorías vacías","Ocasionalmente"]
          },
          { id:"bebidas", label:"Gaseosas y jarabes 🥤", emoji:"🥤",
            desc: "Bebidas azucaradas aportan calorías libres sin micronutrientes y están relacionadas con aumento de peso y riesgo metabólico. Reemplazarlas por agua, infusiones o bebidas sin azúcares añadidos es recomendable.",
            props:["Azúcares añadidos","Riesgo metabólico","Evitar consumo frecuente"]
          }
        ]
      }
    ]
  }
};

/* ---------- UI logic ---------- */

const panelEl = document.getElementById('panel');
const listArea = document.getElementById('listArea');
const drawer = document.getElementById('drawer');
const detailEmoji = document.getElementById('detailEmoji');
const detailName = document.getElementById('detailName');
const detailGroup = document.getElementById('detailGroup');
const detailDesc = document.getElementById('detailDesc');
const detailProps = document.getElementById('detailProps');

function clearList(){
  listArea.innerHTML = '';
  panelEl.querySelector('h2').textContent = 'Selecciona un alimento';
}

/* Show category in panel with groups + clickable items */
function showCategory(catKey){
  const data = DATA[catKey];
  if(!data) return;
  panelEl.querySelector('h2').textContent = data.title;
  panelEl.querySelector('p').textContent = `Selecciona un subgrupo y luego un alimento para ver la ficha detallada.`;
  const container = document.createElement('div');
  container.style.marginTop = '8px';

  data.subgroups.forEach(sub=>{
    const sgTitle = document.createElement('h3');
    sgTitle.textContent = sub.name;
    sgTitle.style.margin = '12px 0 6px';
    sgTitle.style.color = '#00695c';
    sgTitle.style.fontSize = '0.98rem';
    container.appendChild(sgTitle);

    const ul = document.createElement('ul');
    ul.className = 'list-group';
    sub.items.forEach((it, idx)=>{
      const li = document.createElement('li');
      li.setAttribute('role','button');
      li.tabIndex = 0;
      li.innerHTML = `<span class="emoji">${it.emoji || ''}</span><span class="name">${it.label}</span>`;
      li.addEventListener('click', ()=> openDetail(catKey, it.id));
      li.addEventListener('keypress', (e)=>{ if(e.key==='Enter') openDetail(catKey, it.id);});
      ul.appendChild(li);
    });

    container.appendChild(ul);
  });

  listArea.innerHTML = '';
  listArea.appendChild(container);
}

/* Find item by id within category */
function findItem(catKey, itemId){
  const data = DATA[catKey];
  if(!data) return null;
  for(const sub of data.subgroups){
    for(const item of sub.items){
      if(item.id === itemId) return {...item, subgroup: sub.name, catTitle: data.title, catEmoji: data.groupEmoji};
    }
  }
  return null;
}

/* Open drawer with detailed info */
function openDetail(catKey, itemId){
  const it = findItem(catKey, itemId);
  if(!it) return;
  // fill drawer
  detailEmoji.textContent = it.emoji || '';
  detailName.textContent = it.label.replace(/\s*\(.+?\)$/,''); // remove parenthesis for title
  detailGroup.textContent = `${it.subgroup} · ${DATA[catKey].title} ${DATA[catKey].groupEmoji || ''}`;
  detailDesc.textContent = it.desc || 'Descripción no disponible.';
  // props list
  detailProps.innerHTML = '';
  if(Array.isArray(it.props)){
    it.props.forEach(p=>{
      const li = document.createElement('li');
      li.textContent = "• " + p;
      detailProps.appendChild(li);
    });
  }
  // show drawer
  drawer.classList.add('open');
  drawer.setAttribute('aria-hidden','false');
}

/* close drawer */
function closeDrawer(){
  drawer.classList.remove('open');
  drawer.setAttribute('aria-hidden','true');
}

/* on load: show base category (granos) by default */
document.addEventListener('DOMContentLoaded', ()=>{
  showCategory('granos');
});

/* Optional: close drawer on Escape */
document.addEventListener('keydown', (e)=>{
  if(e.key === 'Escape') closeDrawer();
});

</script>

<footer style="
  text-align: center;
  margin-top: 40px;
  padding: 15px;
  font-size: 14px;
  color: #555;
  background: #f0f0f0;
  border-top: 2px solid #ccc;
  font-family: 'Poppins', sans-serif;
">
  Hecho por <strong>Mirko Saravia</strong>
</footer>

</body>
</html>
