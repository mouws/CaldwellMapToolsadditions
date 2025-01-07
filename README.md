<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Commoner Generator</title>

  <!-- ========== GOOGLE FONTS ========== -->
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link 
    href="https://fonts.googleapis.com/css2?family=Almendra&family=Cinzel:wght@400;700&display=swap" 
    rel="stylesheet"
  >

  <style>
    /* ============ BASIC STYLES ============ */
    body {
      margin: 1rem;
      background: #FAF4E0;  /* Parchment-inspired background */
      color: #333;
      font-family: 'Almendra', serif; /* Body text: Almendra */
    }

    h1, h2, h3 {
      font-family: 'Cinzel', serif; /* Headings: Cinzel */
      margin-bottom: 0.5rem;
    }

    .fancy-divider {
      margin: 0.25rem 0 1rem;
      font-size: 1.2rem;
      text-align: center;
    }
    .fancy-divider::after {
      content: "✦ ✧ ✦";
      color: #8B0000;
      letter-spacing: 0.5rem;
    }

    .container {
      max-width: 800px;
      margin: 0 auto;
    }

    label {
      display: inline-block;
      width: 150px;
      margin-right: 8px;
      font-weight: bold;
    }

    select, input[type="text"], input[type="number"] {
      margin-bottom: 1rem;
      padding: 0.2rem;
      font-size: 1rem;
      background-color: #fff9ed; /* Slightly off-white for parchment vibe */
      color: #333;
      border: 1px solid #999;
      font-family: 'Almendra', serif;
    }

    .button {
      background-color: #8B0000; /* A deeper red */
      color: #fff;
      border: none;
      padding: 0.5rem 1rem;
      cursor: pointer;
      font-size: 1rem;
      margin: 0.5rem 0;
      font-family: 'Cinzel', serif;
    }

    .button:hover {
      background-color: #6B0000; /* Darker red on hover */
    }

    .output {
      margin-top: 2rem;
      padding: 1rem;
      border: 1px solid #aaa;
      background-color: #fff5e6; /* Slightly lighter than body background */
      color: #333;
    }

    .ability-scores, .background-info, .race-info {
      margin-bottom: 1rem;
    }

    .footer {
      margin-top: 3rem;
      font-size: 0.9rem;
      color: #666;
      font-family: 'Almendra', serif;
    }

    /* Hide the print button if the output is not visible yet */
    #printButton {
      display: none;
    }

    /* ============= PRINT ONLY THE SHEET =============
       When printing, hide everything except #output. */

    @media print {
      body * {
        visibility: hidden !important; /* Hide everything by default */
      }
      #output,
      #output * {
        visibility: visible !important; /* Show the output content */
      }
      #output {
        position: absolute;
        left: 0;
        top: 0;
        width: 100%;
      }
    }
  </style>
</head>
<body>

<div class="container">
  <h1>Commoner Generator</h1>
  <div class="fancy-divider"></div>
  
  <p>
    Generate a “level 0” commoner with a chosen or random race and background, including 
    ability scores, skill proficiencies, personality traits, and more.
  </p>

  <!-- ============ CHARACTER CREATION FORM ============ -->
  <div id="creation-form">

    <!-- Character Name -->
    <div>
      <label for="name-input">Name:</label>
      <input type="text" id="name-input" placeholder="Enter or randomise" />
      <button class="button" onclick="randomName()">Random</button>
    </div>

    <!-- Race Selection -->
    <div>
      <label for="race-select">Race:</label>
      <select id="race-select">
        <option value="">-- Choose --</option>
      </select>
      <button class="button" onclick="randomRace()">Random</button>
    </div>

    <!-- Background Selection -->
    <div>
      <label for="background-select">Background:</label>
      <select id="background-select">
        <option value="">-- Choose --</option>
      </select>
      <button class="button" onclick="randomBackground()">Random</button>
    </div>

    <!-- Ability Scores -->
    <div>
      <label>Ability Scores:</label><br/>
      <button class="button" onclick="rollAbilityScores()">Roll (4d6 drop lowest)</button>
      <button class="button" onclick="standardArray()">Standard Array (15,14,13,12,10,8)</button>
      <div id="abilities-display" style="margin-top:0.5rem;"></div>
    </div>

    <!-- Generate Character & Full Random -->
    <div>
      <button class="button" onclick="generateCharacter()">Generate Commoner</button>
      <button class="button" onclick="fullRandom()">Full Random</button>
    </div>
  </div>

  <!-- ============ OUTPUT ============ -->
  <div id="output" class="output" style="display: none;">
    <h2>Your Generated Commoner</h2>
    <div class="fancy-divider"></div>

    <div id="charName"></div>
    <div id="charRace"></div>
    <div id="charBackground"></div>
    <div id="charAbilityScores"></div>
    <div id="charProficiencies"></div>
    <div id="charLanguages"></div>
    <div id="charEquipment"></div>

    <hr/>
    <div id="charTraits" style="margin-top:1rem;"></div>
  </div>

  <!-- PRINT BUTTON (will appear once there's a generated character) -->
  <button class="button" id="printButton" onclick="window.print()">Print Commoner</button>

  <!-- ============ OGL NOTICE (FULL) ============ -->
  <div class="footer">
    <h3>Open Game License</h3>
    <p>
      This work includes material taken from the 5e System Reference Document (SRD),
      © 2016, Wizards of the Coast, Inc. and is covered by the 
      <strong>Open Game License, Version 1.0a</strong>. 
     
      <br/><br/>
      <em>
      OPEN GAME LICENSE Version 1.0a

      The following text is the property of Wizards of the Coast, Inc. and is Copyright 
      2000 Wizards of the Coast, Inc ("Wizards"). All Rights Reserved.
      
      1. Definitions: 
      (a)"Contributors" means the copyright and/or trademark owners who have contributed 
      Open Game Content; (b)"Derivative Material" means copyrighted material including 
      derivative works and translations (including into other computer languages), 
      potation, modification, correction, addition, extension, upgrade, improvement, 
      compilation, abridgment or other form in which an existing work may be recast, 
      transformed or adapted; (c) "Distribute" means to reproduce, license, rent, 
      lease, sell, broadcast, publicly display, transmit or otherwise distribute; 
      (d)"Open Game Content" means the game mechanic and includes the methods, 
      procedures, processes and routines to the extent such content does not embody 
      the Product Identity and is an enhancement over the prior art and any additional 
      content clearly identified as Open Game Content by the Contributor, and means 
      any work covered by this License, including translations and derivative works 
      under copyright law, but specifically excludes Product Identity.
      (e) "Product Identity" means product and product line names, logos and identifying 
      marks including trade dress; artifacts; creatures characters; stories, storylines, 
      plots, thematic elements, dialogue, incidents, language, artwork, symbols, designs, 
      depictions, likenesses, formats, poses, concepts, themes and graphic, photographic 
      and other visual or audio representations; names and descriptions of characters, 
      spells, enchantments, personalities, teams, personas, likenesses and special 
      abilities; places, locations, environments, creatures, equipment, magical or 
      supernatural abilities or effects, logos, symbols, or graphic designs; and any 
      other trademark or registered trademark clearly identified as Product identity 
      by the owner of the Product Identity, and which specifically excludes the 
      Open Game Content; (f) "Trademark" means the logos, names, mark, sign, motto, 
      designs that are used by a Contributor to identify itself or its products or 
      the associated products contributed to the Open Game License by the Contributor
      (g) "Use", "Used" or "Using" means to use, Distribute, copy, edit, format, modify, 
      translate and otherwise create Derivative Material of Open Game Content. 
      (h) "You" or "Your" means the licensee in terms of this agreement.
      
      2. The License: This License applies to any Open Game Content that contains a 
      notice indicating that the Open Game Content may only be Used under and in terms 
      of this License. You must affix such a notice to any Open Game Content that you Use. 
      No terms may be added to or subtracted from this License except as described by 
      the License itself. No other terms or conditions may be applied to any Open Game 
      Content distributed using this License.
      
      3. Offer and Acceptance: By Using the Open Game Content You indicate Your acceptance 
      of the terms of this License.
      
      4. Grant and Consideration: In consideration for agreeing to use this License, the 
      Contributors grant You a perpetual, worldwide, royalty-free, non-exclusive license 
      with the exact terms of this License to Use, the Open Game Content.
      
      5. Representation of Authority to Contribute: If You are contributing original material 
      as Open Game Content, You represent that Your Contributions are Your original creation 
      and/or You have sufficient rights to grant the rights conveyed by this License.
      
      6. Notice of License Copyright: You must update the COPYRIGHT NOTICE portion of 
      this License to include the exact text of the COPYRIGHT NOTICE of any Open Game 
      Content You are copying, modifying or distributing, and You must add the title, 
      the copyright date, and the copyright holder's name to the COPYRIGHT NOTICE 
      of any original Open Game Content you Distribute.
      
      7. Use of Product Identity: You agree not to Use any Product Identity, including 
      as an indication as to compatibility, except as expressly licensed in another, 
      independent Agreement with the owner of each element of that Product Identity. 
      You agree not to indicate compatibility or co-adaptability with any Trademark or 
      Registered Trademark in conjunction with a work containing Open Game Content 
      except as expressly licensed in another, independent Agreement with the owner 
      of such Trademark or Registered Trademark. The use of any Product Identity in 
      Open Game Content does not constitute a challenge to the ownership of that 
      Product Identity. The owner of any Product Identity used in Open Game Content 
      shall retain all rights, title and interest in and to that Product Identity.
      
      8. Identification: If you distribute Open Game Content You must clearly indicate 
      which portions of the work that you are distributing are Open Game Content.
      
      9. Updating the License: Wizards or its designated Agents may publish updated 
      versions of this License. You may use any authorized version of this License 
      to copy, modify and distribute any Open Game Content originally distributed under 
      any version of this License.
      
      10. Copy of this License: You MUST include a copy of this License with every copy 
      of the Open Game Content You Distribute.
      
      11. Use of Contributor Credits: You may not market or advertise the Open Game Content 
      using the name of any Contributor unless You have written permission from the 
      Contributor to do so.
      
      12. Inability to Comply: If it is impossible for You to comply with any of the terms 
      of this License with respect to some or all of the Open Game Content due to statute, 
      judicial order, or governmental regulation then You may not Use any Open Game 
      Material so affected.
      
      13. Termination: This License will terminate automatically if You fail to comply with 
      all terms herein and fail to cure such breach within 30 days of becoming aware of 
      the breach. All sublicenses shall survive the termination of this License.
      
      14. Reformation: If any provision of this License is held to be unenforceable, 
      such provision shall be reformed only to the extent necessary to make it enforceable.
      
      15. COPYRIGHT NOTICE  
      Open Game License v 1.0a © 2000, Wizards of the Coast, LLC.  
      System Reference Document 5.1 © 2016, Wizards of the Coast, Inc.; Authors Mike Mearls, 
      Jeremy Crawford, Chris Perkins, Rodney Thompson, Peter Lee, James Wyatt, Robert J. Schwalb, 
      Bruce R. Cordell, Chris Sims, and Steve Townshend, based on original material by 
      E. Gary Gygax and Dave Arneson.
      </em>
    </p>
  </div>
</div>


<script>
/* ==================================================
   ========== LARGE NAME SYSTEM (FIRST + LAST) ======
   ================================================== */

/**
 * RAW_NAMES is the exhaustive list, with NO omissions.
 * This includes southern-inspired, Cajun/French, German,
 * Finnish, and additional/mixed names.
 */
const RAW_NAMES = [
  // --- Southern-Inspired Names ---
  "Abby Sue",
  "Adelaide Rae",
  "Alvin Lee",
  "Annabeth Lou",
  "Arlen James",
  "Bea Mae",
  "Beckett Mose",
  "Belle Jo",
  "Bobby Ray",
  "Bonnie Faye",
  "Buck Harding",
  "Clarabelle",
  "Claudette Pearl",
  "Clinton Dean",
  "Cordelia Grace",
  "Darcy Lynn",
  "Darlene Rose",
  "Delilah Kay",
  "Duke Earle",
  "Ella Beth",
  "Etta Lou",
  "Flannery Kate",
  "Florence Jean",
  "Forrest Wade",
  "Franklin Dale",
  "Freddie Sue",
  "Grady Lance",
  "Hannah Leigh",
  "Harper June",
  "Hollis Reed",
  "Ida Mae",
  "Iris Jeanine",
  "Jebediah \"Jeb\"",
  "Jessaline \"Jess\"",
  "Jolene Pearl",
  "Judd Carter",
  "Kady Lynne",
  "Kinley Grace",
  "Kirby Wyatt",
  "Lambert Clyde",
  "Lavinia Belle",
  "Leona Rae",
  "Lester Hoyt",
  "Lila Rose",
  "Loretta Gale",
  "Louis Hunter",
  "Lucille Anne",
  "Mack Travis",
  "Marlena Jo",
  "Maude Elaine",
  "Memphis Dale",
  "Merrilee Lou",
  "Millie Joyce",
  "Nola Paige",
  "Orabelle Jane",
  "Orin Mac",
  "Patsy Lee",
  "Peyton Vance",
  "Presley Dawn",
  "Reba Quinn",
  "Reuben Clive",
  "Rosalee Lynn",
  "Sadie Beth",
  "Sheridan Clay",
  "Sonny Ralph",
  "Stella Jean",
  "Sully Dean",
  "Susannah Ray",
  "Tanner Royce",
  "Tara Jo",
  "Tate Abel",
  "Tennessee Wells",
  "Tessa Rae",
  "Travis Wade",
  "Trixie Lynn",
  "Tyson Lee",
  "Velma Kay",
  "Vernon Todd",
  "Violet June",
  "Waylon Clark",
  "Waverly Lane",
  "Willodean \"Willie\"",
  "Winnie Belle",
  "Zelda Ruth",
  
  // --- Cajun/French-Inspired Names ---
  "Aimée-Noelle",
  "Alphonsine",
  "Amélie Colette",
  "Antoine Reynaud",
  "Armand Gautier",
  "Beaufort \"Beau\"",
  "Bernadette Delacroix",
  "Camille Dupont",
  "Cécile Marin",
  "Chantal Boudreaux",
  "Charlot Dupuis",
  "Claudine Désirée",
  "Corinne Lajoie",
  "Delphine Robichaux",
  "Édouard Babineaux",
  "Eloise Broussard",
  "Estelle LaFleur",
  "Étienne Dubois",
  "Evangeline Badeaux",
  "Félix Morel",
  "Florent Dupre",
  "Gaëtan Renault",
  "Gervais Bouchard",
  "Honoré Lavigne",
  "Jacques Landry",
  "Jean-Baptiste Robillard",
  "Jean-Luc Martine",
  "Jolie Esmé",
  "Leonie Arcenaux",
  "Lisette Dauphin",
  "Lucette Violette",
  "Lucien Armand",
  "Madelon Artois",
  "Margot Pellerin",
  "Marius Gaspard",
  "Mathilde Perreault",
  "Maurice LeClerc",
  "Odette Marchand",
  "Pierre Lafayette",
  "Remy Roux",
  "Sylvain Thibodeaux",
  "Thierry Gaspard",
  
  // --- German-Inspired Names ---
  "Adelheid Roth",
  "Albert Drechsler",
  "Anneliese Becker",
  "Bastian Jäger",
  "Beate Vogel",
  "Benedikt Kraus",
  "Bernd Baumann",
  "Brigitte Neumann",
  "Carla Meier",
  "Christa Zielke",
  "Dieter Vogel",
  "Dorothea Hauer",
  "Eberhard Zeigler",
  "Edith Schuster",
  "Elias Drescher",
  "Erika Sasse",
  "Ernst Kaufmann",
  "Ferdinand Braun",
  "Frieda Hoffman",
  "Friedrich Schaller",
  "Gerda Mayer",
  "Gerhard Weber",
  "Gertrud Hauck",
  "Giselher Bauer",
  "Gudrun Fischer",
  "Hans König",
  "Heide Rösler",
  "Heinrich Beck",
  "Helga Möbius",
  "Hilde Schaefer",
  "Ingrid Pfaff",
  "Johann Albrecht",
  "Jörg Vogt",
  "Karin Ziegler",
  "Karl Kempf",
  "Katharina Scholz",
  "Konrad Schmitt",
  "Liesel Engel",
  "Marlene Wilke",
  "Otto Kuhl",
  "Sigmund Richter",
  "Stefan Eberhardt",
  "Ulrike Werner",
  
  // --- Finnish-Inspired Names ---
  "Aino Tuulikki",
  "Aleksi Heikkinen",
  "Alina Koivunen",
  "Anja Saarinen",
  "Arto Mikkonen",
  "Eelis Pennanen",
  "Eero Ranta",
  "Elina Kaarina",
  "Emilia Järvinen",
  "Esko Niemi",
  "Hanna Vartiainen",
  "Heikki Pulkkinen",
  "Henna Korhonen",
  "Ilkka Pennanen",
  "Johanna Heikkilä",
  "Joonas Rantala",
  "Juha Partanen",
  "Juho Virtanen",
  "Kaarina Salminen",
  "Kaisa Kurki",
  "Kalevi Mäkinen",
  "Katja Peltonen",
  "Kristiina Väisänen",
  "Lasse Lehtonen",
  "Liisa Erkkilä",
  "Maarit Halonen",
  "Matti Aalto",
  "Miika Nurmi",
  "Mikko Korpi",
  "Niko Hautala",
  "Noora Rinne",
  "Olli Kukkonen",
  "Orvokki Paananen",
  "Paavo Kallio",
  "Petri Vainio",
  "Riikka Mustonen",
  "Saara Piirainen",
  "Sanna Koivu",
  "Seppo Valtonen",
  "Tapio Leppänen",
  "Taru Korpela",
  "Tiina Koski",
  "Tuula Mäkelä",
  "Vilma Pelkonen",

  // --- Additional (Southern, Cajun, German, Finnish) Mix & Misc. ---
  "Alina Reznik",
  "Beck Mullins",
  "Bernard Lamar",
  "Celestine Broussard",
  "Darla Gros",
  "Darrell Louviere",
  "Donovan Lee",
  "Ervin Halvari",
  "Eulalie Chauvin",
  "Felicia Merle",
  "Finn Jääskeläinen",
  "Gaston Lavigne",
  "Gunther Ernst",
  "Heike Näslund",
  "Hilda Lamm",
  "Inka Lampinen",
  "Jaana Pihkala",
  "Jesse-Mae Glover",
  "Kasper Autio",
  "Katrin Müller",
  "Kylah Beth",
  "Lafayette Boudreau",
  "Lavinia Faye",
  "Lola Mignon",
  "Lorelei Stein",
  "Louella Hicks",
  "Luke Monroe",
  "Maija Heiskanen",
  "Manfred Erber",
  "Marceline Arnaud",
  "Marja-Leena Koivu",
  "Mason Wyatt",
  "Mireille Malveaux",
  "Neville Fontenot",
  "Odile Champoux",
  "Oscar Wendel",
  "Pearl Nettles",
  "Priscilla Joplin",
  "Reinhardt Faust",
  "Risto Vaananen",
  "Rudolf Meissner",
  "Ruth Ellen",
  "Sade Rantanen",
  "Sallie June",
  "Severin Vogel",
  "Signe Ollila",
  "Suzette Armond",
  "Timo Harkonen",
  "Toby Luther",
  "Vera Gail",
  "Viktoria Haas",
  "Ville Rautio",
  "Wilhelm Schuster",
  "Winifred Lane"
];

// We'll parse RAW_NAMES to produce separate FIRST_NAMES and LAST_NAMES arrays.
let FIRST_NAMES = [];
let LAST_NAMES = [];

function parseName(fullName) {
  const trimmed = fullName.trim();
  if (!trimmed) return null;
  const parts = trimmed.split(/\s+/);
  if (parts.length === 1) {
    // single word => first name only
    return { first: parts[0].replace(/"/g, ""), last: "" };
  } else {
    const first = parts[0].replace(/"/g, "");
    const last = parts[parts.length - 1].replace(/"/g, "");
    return { first, last };
  }
}

// Build FIRST_NAMES and LAST_NAMES from RAW_NAMES
RAW_NAMES.forEach(entry => {
  const parsed = parseName(entry);
  if (parsed) {
    if (parsed.first) FIRST_NAMES.push(parsed.first);
    if (parsed.last) LAST_NAMES.push(parsed.last);
  }
});

/* ==================================================
   ========== SRD-BASED DATA (SIMPLIFIED) ==========
   ================================================== */

/* --- Races --- */
const RACES = [
  {
    name: "Dwarf",
    abilityScoreBonuses: { CON: 2 },
    speed: 25,
    features: [
      "Darkvision (60 ft)",
      "Dwarven Resilience (adv. on poison saves)",
      "Dwarven Combat Training",
      "Stonecunning"
    ],
    languages: ["Common", "Dwarvish"]
  },
  {
    name: "Elf",
    abilityScoreBonuses: { DEX: 2 },
    speed: 30,
    features: [
      "Darkvision (60 ft)",
      "Keen Senses (proficiency in Perception)",
      "Fey Ancestry (adv. on charm saves)",
      "Trance"
    ],
    languages: ["Common", "Elvish"]
  },
  {
    name: "Halfling",
    abilityScoreBonuses: { DEX: 2 },
    speed: 25,
    features: [
      "Lucky (reroll 1s on attack/ability/saving throws)",
      "Brave (adv. on fear saves)",
      "Halfling Nimbleness"
    ],
    languages: ["Common", "Halfling"]
  },
  {
    name: "Human",
    abilityScoreBonuses: {
      STR: 1, DEX: 1, CON: 1, INT: 1, WIS: 1, CHA: 1
    },
    speed: 30,
    features: [],
    languages: ["Common", "One Extra Language of your choice"]
  },
  {
    name: "Dragonborn",
    abilityScoreBonuses: { STR: 2, CHA: 1 },
    speed: 30,
    features: [
      "Draconic Ancestry",
      "Breath Weapon",
      "Damage Resistance (based on ancestry)"
    ],
    languages: ["Common", "Draconic"]
  },
  {
    name: "Gnome",
    abilityScoreBonuses: { INT: 2 },
    speed: 25,
    features: [
      "Darkvision (60 ft)",
      "Gnome Cunning (adv. on mental saves vs. magic)"
    ],
    languages: ["Common", "Gnomish"]
  },
  {
    name: "Half-Elf",
    abilityScoreBonuses: { CHA: 2, PLUS_TWO_OTHERS: 1 },
    speed: 30,
    features: [
      "Darkvision (60 ft)",
      "Fey Ancestry (adv. on charm saves)",
      "Skill Versatility (proficiency in two skills)"
    ],
    languages: ["Common", "Elvish", "One Extra Language"]
  },
  {
    name: "Half-Orc",
    abilityScoreBonuses: { STR: 2, CON: 1 },
    speed: 30,
    features: [
      "Darkvision (60 ft)",
      "Relentless Endurance (once per long rest)",
      "Savage Attacks"
    ],
    languages: ["Common", "Orc"]
  },
  {
    name: "Tiefling",
    abilityScoreBonuses: { CHA: 2, INT: 1 },
    speed: 30,
    features: [
      "Darkvision (60 ft)",
      "Hellish Resistance (resist fire)",
      "Infernal Legacy"
    ],
    languages: ["Common", "Infernal"]
  }
];

/* --- Backgrounds, with full featureDesc + nicknames (1/5 chance) --- */
const BACKGROUNDS = [
  {
    name: "Acolyte",
    skillProficiencies: ["Insight", "Religion"],
    languages: ["Two of your choice"],
    equipment: [
      "Holy symbol",
      "Prayer book or prayer wheel",
      "5 sticks of incense",
      "Vestments",
      "Set of common clothes",
      "Belt pouch containing 15 gp"
    ],
    feature: "Shelter of the Faithful",
    featureDesc: "As an Acolyte, you command the respect of those who share your faith. You can perform religious ceremonies and can call upon fellow adherents for shelter and support, though you must uphold the practices of your religion in return.",
    nicknames: ["Chant", "Chapel", "Faith", "Lumen", "Reverie"]
  },
  {
    name: "Criminal/Spy",
    skillProficiencies: ["Deception", "Stealth"],
    equipment: [
      "Crowbar",
      "Set of dark common clothes including hood",
      "Belt pouch containing 15 gp"
    ],
    feature: "Criminal Contact",
    featureDesc: "You have a reliable, trustworthy contact who acts as your liaison to a network of other criminals. You know how to get messages to and from your contact, even over great distances.",
    nicknames: ["Shade", "Slick", "Pockets", "Rook", "Whisper"]
  },
  {
    name: "Folk Hero",
    skillProficiencies: ["Animal Handling", "Survival"],
    equipment: [
      "Set of artisan's tools (one of your choice)",
      "Shovel",
      "Iron pot",
      "Set of common clothes",
      "Belt pouch containing 10 gp"
    ],
    feature: "Rustic Hospitality",
    featureDesc: "You fit in among common folk easily. They will shield you from the law or anyone else searching for you, though they will not risk their lives for you. In return, you must come to their aid in times of need.",
    nicknames: ["Buck", "Rusty", "Spotts", "Sarge", "Oakie"]
  },
  {
    name: "Guild Artisan (Guild Merchant)",
    skillProficiencies: ["Insight", "Persuasion"],
    languages: ["One of your choice"],
    equipment: [
      "Set of artisan's tools (one of your choice)",
      "Letter of introduction from your guild",
      "Set of traveller's clothes",
      "Belt pouch containing 15 gp"
    ],
    feature: "Guild Membership",
    featureDesc: "Your guild offers you lodging and food, and you can receive legal and political support from them when needed. Guild members will often support you if it does not risk their own standing.",
    nicknames: ["Hammer", "Chisel", "Scribe", "Barter", "Ledger"]
  },
  {
    name: "Hermit",
    skillProficiencies: ["Medicine", "Religion"],
    equipment: [
      "Scroll case stuffed full of notes from your studies or prayers",
      "Winter blanket",
      "Set of common clothes",
      "Herbalism kit",
      "5 gp"
    ],
    feature: "Discovery",
    featureDesc: "Your seclusion allowed you to uncover a unique truth about the cosmos—an insight that may shape your destiny. Some might not accept your findings; you must decide how to share them, if at all.",
    nicknames: ["Quiet", "Wisp", "Moony", "Muse", "Cloister"]
  },
  {
    name: "Noble",
    skillProficiencies: ["History", "Persuasion"],
    equipment: [
      "Set of fine clothes",
      "Signet ring",
      "Scroll of pedigree",
      "Purse containing 25 gp"
    ],
    feature: "Position of Privilege",
    featureDesc: "You carry the authority of your noble birth. Commoners treat you with respect, and other nobles see you as a peer. You can often secure an audience with the local ruler if you need to.",
    nicknames: ["Lordy", "Duchess", "Grace", "Count", "Regal"]
  },
  {
    name: "Outlander",
    skillProficiencies: ["Athletics", "Survival"],
    languages: ["One of your choice"],
    equipment: [
      "Staff",
      "Hunting trap",
      "Trophy from an animal you killed",
      "Set of traveller's clothes",
      "Belt pouch containing 10 gp"
    ],
    feature: "Wanderer",
    featureDesc: "You have an excellent memory for geography and can always recall the general layout of terrain, settlements, and other features around you. You can find food and fresh water for yourself and up to five other people each day.",
    nicknames: ["Tracks", "Wild", "Hawk", "Cairn", "Scout"]
  },
  {
    name: "Sage",
    skillProficiencies: ["Arcana", "History"],
    equipment: [
      "Bottle of black ink",
      "Quill",
      "Small knife",
      "Letter from a dead colleague posing a question you have not yet been able to answer",
      "Set of common clothes",
      "Belt pouch containing 10 gp"
    ],
    feature: "Researcher",
    featureDesc: "When you attempt to learn or recall a piece of lore, if you don't know that information, you often know where and from whom you can obtain it. Your DM might rule that the knowledge you seek is secreted away or that it simply cannot be found.",
    nicknames: ["Quill", "Stacks", "Owl", "Doc", "Booker"]
  },
  {
    name: "Sailor",
    skillProficiencies: ["Athletics", "Perception"],
    equipment: [
      "Belaying pin (club)",
      "50 feet of silk rope",
      "Lucky charm",
      "Set of common clothes",
      "Belt pouch containing 10 gp"
    ],
    feature: "Ship's Passage",
    featureDesc: "You can secure free passage on a sailing ship for yourself and your adventuring companions. In return, you must assist the crew during the voyage. You can't be certain of a schedule, however.",
    nicknames: ["Salty", "Anchor", "Rudder", "Knotty", "Seabreeze"]
  },
  {
    name: "Soldier",
    skillProficiencies: ["Athletics", "Intimidation"],
    equipment: [
      "Insignia of rank",
      "Trophy taken from a fallen enemy",
      "Set of bone dice or deck of cards",
      "Set of common clothes",
      "Belt pouch containing 10 gp"
    ],
    feature: "Military Rank",
    featureDesc: "You have a military rank from your career as a soldier. Soldiers loyal to your former military organisation still recognise your authority and influence, and they defer to you if they are of a lower rank.",
    nicknames: ["Sarge", "Brass", "Boots", "Vets", "Gunner"]
  },
  {
    name: "Urchin",
    skillProficiencies: ["Sleight of Hand", "Stealth"],
    equipment: [
      "Small knife",
      "Map of the city you grew up in",
      "Pet mouse",
      "Token to remember your parents by",
      "Set of common clothes",
      "Belt pouch containing 10 gp"
    ],
    feature: "City Secrets",
    featureDesc: "You know the secret patterns and flow to cities and can find passages through the urban sprawl that others would miss. You and your companions can travel between any two locations in the city twice as fast as your speed would normally allow.",
    nicknames: ["Rat", "Stray", "Alley", "Gutters", "Pebbles"]
  },
  {
    name: "Tanner",
    skillProficiencies: ["Survival", "Nature"],
    languages: [],
    equipment: [
      "A small set of tanning tools",
      "A sturdy apron",
      "A cured hide you’re particularly proud of",
      "A set of common clothes",
      "Belt pouch containing 5 gp"
    ],
    feature: "Leatherwise",
    featureDesc: "You instantly identify the quality and source of hides or leather goods. You can also typically find buyers for raw hides or furs, even in remote settlements, thanks to your trade connections.",
    nicknames: ["Hide", "Skins", "Tannic", "Pelt", "Sinew"]
  },
  {
    name: "Alchemist",
    skillProficiencies: ["Arcana", "Medicine"],
    languages: [],
    equipment: [
      "An alchemist’s kit (including flasks, mortar & pestle)",
      "A weathered notebook filled with formulae",
      "A set of common clothes stained with reagents",
      "Belt pouch containing 10 gp"
    ],
    feature: "Experimental Brewer",
    featureDesc: "Local apothecaries or herbalists often grant you discounts or small freebies, hoping your next discovery will be lucrative. You can usually identify common magical or chemical substances on sight.",
    nicknames: ["Vial", "Fume", "Brew", "Pestle", "Tonic"]
  },
  {
    name: "Miner",
    skillProficiencies: ["Athletics", "Perception"],
    languages: [],
    equipment: [
      "A pickaxe",
      "A lantern and a flask of oil",
      "A small sample of precious ore (souvenir)",
      "Sturdy boots and a set of rugged common clothes",
      "Belt pouch containing 10 gp"
    ],
    feature: "Tunnel Instinct",
    featureDesc: "You have an uncanny sense for detecting structural weaknesses underground and can often identify if a tunnel is stable. Other miners or dwarves typically trust your word on subterranean matters.",
    nicknames: ["Pick", "Sparky", "Ore", "Dusty", "Coal"]
  },
  {
    name: "Beast Tamer",
    skillProficiencies: ["Animal Handling", "Nature"],
    languages: [],
    equipment: [
      "A length of rope",
      "A small, carved token of a favourite beast",
      "A supply of dried meat or treats",
      "A set of simple outdoor attire",
      "Belt pouch containing 5 gp"
    ],
    feature: "Animal Rapport",
    featureDesc: "You have a calm and trustworthy air that most domestic animals respond to. Locals often ask you for help handling skittish or aggressive beasts.",
    nicknames: ["Buck", "Wrangler", "Whistle", "Critter", "Hoof"]
  },
  {
    name: "Chandler",
    skillProficiencies: ["Persuasion", "Investigation"],
    languages: [],
    equipment: [
      "A small block of wax",
      "A candle mould and a tinderbox",
      "A ledger tracking your sales",
      "A set of plain but neat clothes",
      "Belt pouch containing 7 gp"
    ],
    feature: "Keeper of Light",
    featureDesc: "You know how to create serviceable candles and have a knack for sourcing cheap tallow or beeswax. Townsfolk trust you not to leave them in the dark.",
    nicknames: ["Candle", "Wax", "Wick", "Tallow", "Glow"]
  },
  {
    name: "Cooper",
    skillProficiencies: ["History", "Sleight of Hand"], 
    languages: [],
    equipment: [
      "A cooper’s hammer and a ring of iron hoops",
      "A small cask you’ve crafted yourself",
      "A set of serviceable working clothes",
      "Belt pouch containing 10 gp"
    ],
    feature: "Well-Bound Containers",
    featureDesc: "You know how to properly seal barrels and crates. Merchants who value secure shipping often appreciate your skills, and you can spot tampering at a glance.",
    nicknames: ["Barrel", "Hoops", "Ring", "Crater", "Tapper"]
  },
  {
    name: "Cartwright",
    skillProficiencies: ["Carpenter’s Tools (use ‘Tool Proficiency’ in your system)", "History"],
    languages: [],
    equipment: [
      "A small woodworking toolkit",
      "A miniature model cart of your own design",
      "A set of sturdy common clothes",
      "Belt pouch containing 10 gp"
    ],
    feature: "Roadwise",
    featureDesc: "Having built and repaired carts, you understand travel and terrain intricately. You often prevent breakdowns or expedite journeys by advising on maintenance and routes.",
    nicknames: ["Axle", "Spokes", "Wheel", "Lumber", "Waggon"]
  },
  {
    name: "Scribe",
    skillProficiencies: ["History", "Investigation"],
    languages: ["One of your choice"], 
    equipment: [
      "A quill and a small jar of ink",
      "A sheaf of parchment",
      "A personal log of your scribblings",
      "A set of simple but tidy clothes",
      "Belt pouch containing 5 gp"
    ],
    feature: "Academic Access",
    featureDesc: "Your skills as a scribe grant you easier access to archives, libraries, and private collections. Scholars trust you to handle delicate documents.",
    nicknames: ["Quill", "Ink", "Scroll", "Lexi", "Page"]
  },
  {
    name: "Glassblower",
    skillProficiencies: ["Arcana", "Perception"],
    languages: [],
    equipment: [
      "A small glassblowing pipe",
      "A carefully wrapped glass trinket of your own design",
      "A sturdy pair of goggles",
      "A set of common clothes",
      "Belt pouch containing 5 gp"
    ],
    feature: "Fragile Eye",
    featureDesc: "Your keen eye discerns flaws and cracks in glass and crystalline materials. Artisans and collectors seek your expertise to judge delicate wares.",
    nicknames: ["Fragile", "Pane", "Spark", "Torch", "Crystal"]
  },
  {
    name: "Jeweller",
    skillProficiencies: ["Insight", "Sleight of Hand"],
    languages: [],
    equipment: [
      "A loupe or small magnifying lens",
      "A pouch of assorted uncut gemstones (low value)",
      "A set of smart but modest clothes",
      "Belt pouch containing 15 gp"
    ],
    feature: "Stone Appraisal",
    featureDesc: "You can swiftly gauge the authenticity and approximate value of gems or precious metals. Wealthy patrons may hire you to inspect prized possessions.",
    nicknames: ["Shine", "Facet", "Gem", "Pearl", "Rocky"]
  },
  {
    name: "Shipwright",
    skillProficiencies: ["Athletics", "Carpenter’s Tools (use ‘Tool Proficiency’ in your system)"],
    languages: [],
    equipment: [
      "A small mallet and chisel",
      "A piece of driftwood with an etched diagram",
      "A set of rugged clothes",
      "Belt pouch containing 10 gp"
    ],
    feature: "Maritime Specialist",
    featureDesc: "Ports and ship captains respect your experience. You can find work or passage aboard ships if you assist with repairs or related tasks. You can also often determine a vessel’s seaworthiness by sight.",
    nicknames: ["Timbers", "Hull", "Dock", "Anchor", "Mast"]
  },
  {
    name: "Bowyer/Fletcher",
    skillProficiencies: ["Nature", "Perception"],
    languages: [],
    equipment: [
      "A bundle of feathers and arrow shafts",
      "A small bowyer’s toolkit",
      "A set of workable common clothes",
      "Belt pouch containing 5 gp"
    ],
    feature: "Arrow Precision",
    featureDesc: "You can craft serviceable arrows from natural materials and assess the quality of bows and arrows at a glance. Hunters might hire you for your craft and expertise.",
    nicknames: ["Arrow", "Feather", "Quiver", "Bowstring", "Fletch"]
  },
  {
    name: "Cobbler",
    skillProficiencies: ["Sleight of Hand", "Insight"],
    languages: [],
    equipment: [
      "A cobbler’s kit (awl, hammer, leather patches)",
      "A well-worn pair of boots you’ve crafted",
      "A set of common clothes",
      "Belt pouch containing 8 gp"
    ],
    feature: "Sure-Footed",
    featureDesc: "You excel at fitting boots and identifying footprints. Travellers appreciate your skill, and you can sometimes track individuals by their unique footwear.",
    nicknames: ["Boots", "Soles", "Stitch", "Tap", "Tread"]
  },
  {
    name: "Farmer",
    skillProficiencies: ["Animal Handling", "Nature"],
    languages: [],
    equipment: [
      "A hoe or sickle",
      "A small pouch of seeds",
      "A wide-brimmed straw hat",
      "A set of sturdy clothes",
      "Belt pouch containing 5 gp"
    ],
    feature: "Local Harvest",
    featureDesc: "You know how to grow and gather simple crops and can usually secure shelter or trade among rural folk who respect your honest labour.",
    nicknames: ["Seed", "Haystack", "Plow", "Row", "Scythe"]
  },
  {
    name: "Fisher",
    skillProficiencies: ["Survival", "Perception"],
    languages: [],
    equipment: [
      "A simple fishing rod or hand line",
      "A small tackle box",
      "A net",
      "A set of waterproof boots",
      "Belt pouch containing 5 gp"
    ],
    feature: "Waterside Hospitality",
    featureDesc: "Those living near bodies of water recognise your knack for fishing. You can often secure a day’s rations for yourself and one or two companions, and fishers in other regions are friendly to you.",
    nicknames: ["Fin", "Hook", "Line", "Nets", "Minnow"]
  }
];

/* ==================================================
   ========== PERSONALITY TRAITS (VAST) =============
   We will pick exactly 2 random traits from this list.
   ================================================== */
const PERSONALITY_TRAITS = [
  "I always see the best in people, even if they don’t deserve it.",
  "I am suspicious of new faces and stick to familiar companions.",
  "I laugh loudly and often, even at my own jokes.",
  "I hum or sing quietly whenever I'm nervous.",
  "I keep small trinkets from my travels and love to show them off.",
  "I have a habit of forgetting names but remembering faces.",
  "I firmly believe in fate and destiny, reading signs everywhere.",
  "I can’t stand the sight of suffering and rush to help those in need.",
  "I hoard secrets like gold and use them to my advantage.",
  "I’m always tidying up, fussing over details even when it’s unnecessary.",
  "I prefer the company of animals to people and often mimic their sounds.",
  "I fixate on a single goal and can become oblivious to other issues.",
  "I daydream about a grand future, sometimes missing important details.",
  "I rarely speak, but when I do, it's usually cutting or direct.",
  "I record everything I see in a small notebook—my memory is precious.",
  "I have strong opinions on art and culture, ignoring practicalities.",
  "I offer unsolicited advice to everyone, whether they ask or not.",
  "I secretly enjoy meddling in other people's affairs to 'help' them.",
  "I become giddy at the prospect of treasure or hidden knowledge.",
  "I am extremely superstitious, performing little rituals constantly.",
  "I avoid direct conflict if I can, preferring to talk my way out.",
  "I try to turn every experience into a grand story or ballad.",
  "I fuss about cleanliness, my gear must always be spotless.",
  "I love discussing philosophy, even if others find it boring.",
  "I feel most alive on the move, restless if I stay in one place long.",
  "I never turn down a good gamble or game of chance.",
  "I see omens in everyday happenings and trust them over logic.",
  "I have an elaborate greeting ritual that I force upon new acquaintances.",
  "I trust animals' judgement of people more than my own.",
  "I keep a strict code of personal honour and won’t break it for anyone.",
  "I love tall tales and exaggerate my own feats for dramatic effect.",
  "I’m reluctant to share personal details, no matter how mundane.",
  "I collect jokes and riddles, testing them on unsuspecting strangers.",
  "I view cynicism as a shield, sarcasm as my chosen weapon.",
  "I can't keep track of time unless I narrate my day out loud.",
  "I have an intense personal rivalry with a close friend or sibling.",
  "I’m fiercely protective of children and the weak, stepping in instantly.",
  "I take minor slights personally and brood over them for days.",
  "I’m fascinated by the arcane, even if I don’t fully understand it.",
  "I can’t resist a friendly wager, even if it’s something I might lose.",
  "I hold grudges far too long, plotting petty revenges.",
  "I deeply admire heroes and strive to become like them someday.",
  "I tease friends with gentle barbs and can’t resist a playful jab.",
  "I am obsessed with collecting curiosities or random knick-knacks.",
  "I hold my faith above all else, quietly praying in every spare moment.",
  "I love performing in front of people—dancing, singing, or oratory.",
  "I see potential threats everywhere and watch my back constantly.",
  "I talk to inanimate objects when lonely, expecting no reply.",
  "I share my food or drink with those less fortunate without hesitation.",
  "I’m easily bored and seek constant stimulation or excitement.",
  "I have a warm, hearty laugh that instantly sets others at ease.",
  "I’ll do almost anything for a good story, even if it puts me at risk.",
  "I rarely show emotion, keeping a stoic mask in all situations.",
  "I can’t stand idleness, fidgeting or pacing if not kept busy.",
  "I always look impeccably groomed, taking pride in my appearance.",
  "I love mysteries and rumours, prying into others’ business for clues.",
  "I speak in grandiose, flowery language that borders on ridiculous.",
  "I resent authority, ignoring orders unless they serve my interest.",
  "I have an odd collection of lucky charms that I never leave behind.",
  "I become starstruck by famous warriors, nobles, or wizards.",
  "I play with any fire source absentmindedly, enthralled by the flames.",
  "I can’t resist a sob story, giving away resources I barely have.",
  "I’m easily distracted by anything unusual or shiny, losing focus.",
  "I brag about my modest accomplishments as if they’re legendary."
];

/* ==================================================
   ========== PAGE INITIALISATION ==========
   ================================================== */

window.addEventListener("DOMContentLoaded", () => {
  populateSelect("race-select", RACES.map(r => r.name));
  populateSelect("background-select", BACKGROUNDS.map(b => b.name));
});

function populateSelect(selectId, optionsArray) {
  const selectElem = document.getElementById(selectId);
  optionsArray.forEach(opt => {
    const optionEl = document.createElement("option");
    optionEl.value = opt;
    optionEl.innerText = opt;
    selectElem.appendChild(optionEl);
  });
}

/* ==================================================
   ========== RANDOMISATION + HELPER FUNCTIONS =======
   ================================================== */

function roll4d6DropLowest() {
  let scores = [];
  for (let i = 0; i < 6; i++) {
    let rolls = [0, 0, 0, 0].map(() => Math.floor(Math.random() * 6) + 1);
    rolls.sort((a, b) => a - b);
    rolls.shift(); // drop the lowest
    const sum = rolls.reduce((acc, val) => acc + val, 0);
    scores.push(sum);
  }
  return scores;
}

function getStandardArray() {
  return [15, 14, 13, 12, 10, 8];
}

function abilityMod(score) {
  return Math.floor((score - 10) / 2);
}

function pickRandom(array) {
  return array[Math.floor(Math.random() * array.length)];
}

/**
 * Picks 'count' distinct random items from 'array'.
 * If 'count' > array.length, it picks as many as possible uniquely.
 */
function pickMultipleDistinct(array, count) {
  const copy = [...array];
  const result = [];
  if (count > copy.length) count = copy.length;
  for (let i = 0; i < count; i++) {
    const index = Math.floor(Math.random() * copy.length);
    result.push(copy[index]);
    copy.splice(index, 1);
  }
  return result;
}

/* ==================================================
   ========== NAME HANDLERS =========================
   ================================================== */

function randomName() {
  if (FIRST_NAMES.length === 0 || LAST_NAMES.length === 0) {
    document.getElementById("name-input").value = "Nameless Soul";
    return;
  }
  const first = pickRandom(FIRST_NAMES);
  const last = pickRandom(LAST_NAMES);
  document.getElementById("name-input").value = first + " " + last;
}

/* ==================================================
   ========== RACE & BACKGROUND RANDOMISERS =========
   ================================================== */

function randomRace() {
  const randomPick = pickRandom(RACES).name;
  document.getElementById("race-select").value = randomPick;
}

function randomBackground() {
  const randomPick = pickRandom(BACKGROUNDS).name;
  document.getElementById("background-select").value = randomPick;
}

/* ==================================================
   ========== ABILITY SCORE HANDLERS ================
   ================================================== */
let currentScores = [10,10,10,10,10,10]; // default

function rollAbilityScores() {
  currentScores = roll4d6DropLowest();
  displayScores();
}

function standardArray() {
  currentScores = getStandardArray();
  displayScores();
}

function displayScores() {
  const container = document.getElementById("abilities-display");
  const labels = ["STR", "DEX", "CON", "INT", "WIS", "CHA"];
  let html = "";
  currentScores.forEach((sc, i) => {
    const mod = abilityMod(sc);
    const modPrefix = mod >= 0 ? "+" : "";
    html += `${labels[i]}: <strong>${sc}</strong> (${modPrefix}${mod})<br/>`;
  });
  container.innerHTML = html;
}

/* ==================================================
   ========== CHARACTER GENERATION ==================
   ================================================== */

function parseUserName(fullInput) {
  const parts = fullInput.trim().split(/\s+/);
  let firstName = "";
  let lastName = "";
  if (parts.length === 1) {
    firstName = parts[0];
  } else if (parts.length > 1) {
    firstName = parts[0];
    lastName = parts[parts.length - 1];
  }
  return { firstName, lastName };
}

function generateCharacter() {
  // 1. Name: parse the user’s typed input
  const rawName = document.getElementById("name-input").value.trim() || "Unnamed Hero";
  let { firstName, lastName } = parseUserName(rawName);

  // 2. Get race data
  const raceSelected = document.getElementById("race-select").value;
  const raceData = RACES.find(r => r.name === raceSelected);
  if (!raceData) {
    alert("Please select a race.");
    return;
  }

  // 3. Get background data
  const bgSelected = document.getElementById("background-select").value;
  const bgData = BACKGROUNDS.find(b => b.name === bgSelected);
  if (!bgData) {
    alert("Please select a background.");
    return;
  }

  // 4. Possibly add a nickname (1/5 chance)
  if (bgData.nicknames && bgData.nicknames.length > 0) {
    if (Math.random() < 0.2) {
      const nick = pickRandom(bgData.nicknames);
      // Insert it in "quotes" after the first name
      firstName += ` "${nick}"`;
    }
  }

  // 5. Apply race ability score bonuses
  let finalScores = [...currentScores];
  const ABIL_ORDER = ["STR", "DEX", "CON", "INT", "WIS", "CHA"];

  for (const [ability, bonus] of Object.entries(raceData.abilityScoreBonuses)) {
    if (ability === "PLUS_TWO_OTHERS") {
      // For Half-Elf, typically +1 to two abilities of choice
      // We'll default to DEX & CON if no manual choice is provided
      const dexIndex = ABIL_ORDER.indexOf("DEX");
      const conIndex = ABIL_ORDER.indexOf("CON");
      finalScores[dexIndex] += 1;
      finalScores[conIndex] += 1;
    } else {
      const index = ABIL_ORDER.indexOf(ability);
      if (index !== -1) {
        finalScores[index] += bonus;
      }
    }
  }

  // 6. Personality Traits: pick exactly 2 from the big list
  const chosenTraits = pickMultipleDistinct(PERSONALITY_TRAITS, 2);

  // 7. Compile details
  const fullName = lastName ? firstName + " " + lastName : firstName;

  const featuresList = raceData.features.length
    ? raceData.features.map(feat => `- ${feat}`).join("<br/>")
    : "None";
  const raceLangs = raceData.languages.join(", ");

  const bgEquipment = bgData.equipment.map(eq => `- ${eq}`).join("<br/>");
  const bgSkills = bgData.skillProficiencies
    ? bgData.skillProficiencies.join(", ")
    : "None";
  let bgLangs = (bgData.languages && bgData.languages.length > 0)
    ? bgData.languages.join(", ")
    : "None";

  // 8. Show final output
  const outputDiv = document.getElementById("output");
  outputDiv.style.display = "block";
  outputDiv.classList.add("generated"); // triggers the print button to appear

  document.getElementById("charName").innerHTML = `<h3>Name: ${fullName}</h3>`;
  document.getElementById("charRace").innerHTML = `<strong>Race:</strong> ${raceData.name} 
    <br/><em>Speed:</em> ${raceData.speed} ft 
    <br/><em>Features:</em><br/> ${featuresList}`;
  
  // Display the short feature name plus the full explanation
  document.getElementById("charBackground").innerHTML = `
    <strong>Background:</strong> ${bgData.name}<br/>
    <em>Skill Proficiencies:</em> ${bgSkills}<br/>
    <em>Equipment:</em><br/>${bgEquipment}<br/>
    <em>Feature:</em> <strong>${bgData.feature}</strong><br/>
    <small>${bgData.featureDesc}</small>
  `;

  document.getElementById("charAbilityScores").innerHTML = formatAbilityScores(finalScores);
  document.getElementById("charProficiencies").innerHTML = `<strong>Proficiencies (Level 0, from Race/Background only):</strong>
    <br/>Background Skills: ${bgSkills}`;
  document.getElementById("charLanguages").innerHTML = `<strong>Languages:</strong>
    <br/>From Race: ${raceLangs}
    <br/>From Background: ${bgLangs}`;
  document.getElementById("charEquipment").innerHTML = `<strong>Starting Equipment (from Background):</strong><br/>${bgEquipment}`;

  // Display the personality traits
  const traitsHTML = `
    <h3>Personality Traits</h3>
    <ul>
      <li>${chosenTraits[0]}</li>
      <li>${chosenTraits[1]}</li>
    </ul>
  `;
  document.getElementById("charTraits").innerHTML = traitsHTML;
}

function formatAbilityScores(scoresArr) {
  const labels = ["STR", "DEX", "CON", "INT", "WIS", "CHA"];
  let output = "<strong>Final Ability Scores (Level 0):</strong><br/>";
  scoresArr.forEach((sc, i) => {
    const mod = abilityMod(sc);
    const modSign = mod >= 0 ? "+" : "";
    output += `${labels[i]}: <strong>${sc}</strong> (${modSign}${mod})<br/>`;
  });
  return output;
}

/* ==================================================
   ========== FULL RANDOM FUNCTION ==================
   ================================================== */
function fullRandom() {
  randomName();
  randomRace();
  randomBackground();
  rollAbilityScores();
  generateCharacter();
}
</script>

</body>
</html>
