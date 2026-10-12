<!DOCTYPE html>
<html lang="en" class="h-full bg-slate-50">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Globalisation Townhall: Perspectives & Trade-offs</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            darkMode: 'class',
            theme: {
                extend: {
                    colors: {
                        brand: {
                            50: '#f0f9ff',
                            100: '#e0f2fe',
                            500: '#0284c7',
                            600: '#0369a1',
                            700: '#075985',
                            800: '#0c4a6e',
                            900: '#0f172a'
                        }
                    },
                    fontFamily: {
                        sans: ['Inter', 'system-ui', 'sans-serif']
                    }
                }
            }
        }
    </script>
    <!-- Lucide Icons -->
    <script src="https://unpkg.com/lucide@latest"></script>
    <!-- Chart.js for Synthesis Radar -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <!-- Canvas Confetti -->
    <script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.6.0/dist/confetti.browser.min.js"></script>
    <style>
        /* Custom scrollbar & pulse animations */
        ::-webkit-scrollbar { width: 8px; }
        ::-webkit-scrollbar-track { background: #f1f5f9; }
        ::-webkit-scrollbar-thumb { background: #cbd5e1; border-radius: 4px; }
        ::-webkit-scrollbar-thumb:hover { background: #94a3b8; }
        
        @keyframes floatSlow {
            0%, 100% { transform: translateY(0px); }
            50% { transform: translateY(-6px); }
        }
        .animate-float { animation: floatSlow 4s ease-in-out infinite; }
    </style>
</head>
<body class="h-full text-slate-800 flex flex-col font-sans selection:bg-sky-100 selection:text-sky-900">

    <header class="bg-slate-900 text-white shadow-md sticky top-0 z-50 border-b border-slate-800">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 h-16 flex items-center justify-between">
            <div class="flex items-center space-x-3 cursor-pointer" onclick="navigateTo('hub')">
                <div class="w-10 h-10 rounded-xl bg-gradient-to-tr from-sky-500 to-indigo-600 flex items-center justify-center font-bold text-lg shadow-lg">
                    <i data-lucide="landmark" class="w-6 h-6 text-white"></i>
                </div>
                <div>
                    <h1 class="text-lg font-bold tracking-tight leading-none text-slate-100">Globalisation Townhall</h1>
                    <p class="text-xs text-sky-400 font-medium">Sec 3 G3 Social Studies Simulation</p>
                </div>
            </div>

            <!-- Header Action Controls -->
            <div class="flex items-center space-x-2 sm:space-x-4">
                <!-- Navigation Buttons -->
                <button onclick="navigateTo('hub')" class="p-2 text-slate-300 hover:text-white hover:bg-slate-800 rounded-lg transition" title="Stakeholder Hub">
                    <i data-lucide="users" class="w-5 h-5"></i>
                </button>
                <button onclick="navigateTo('glossary')" class="p-2 text-slate-300 hover:text-white hover:bg-slate-800 rounded-lg transition" title="SS Concept Glossary">
                    <i data-lucide="book-open" class="w-5 h-5"></i>
                </button>
                <button onclick="navigateTo('synthesis')" class="bg-gradient-to-r from-sky-500 to-indigo-600 hover:from-sky-600 hover:to-indigo-700 text-white text-xs font-semibold px-3.5 py-2 rounded-lg transition flex items-center space-x-1.5 shadow-md">
                    <i data-lucide="scale" class="w-4 h-4"></i>
                    <span class="hidden sm:inline">Townhall Finale</span>
                </button>
            </div>
        </div>
    </header>

    <!-- Main Dynamic View Container -->
    <main id="main-container" class="flex-1 max-w-7xl w-full mx-auto p-4 sm:p-6 lg:p-8">
        <!-- Views will be injected via JS -->
    </main>

    <!-- Custom Modal Container -->
    <div id="modal-overlay" class="fixed inset-0 bg-slate-900/60 backdrop-blur-sm z-50 hidden flex items-center justify-center p-4">
        <div id="modal-box" class="bg-white dark:bg-slate-900 rounded-2xl shadow-2xl max-w-lg w-full p-6 border border-slate-100 transform transition-all scale-95 opacity-0">
            <div class="flex items-start justify-between mb-4">
                <div class="flex items-center space-x-3">
                    <div id="modal-icon-bg" class="w-10 h-10 rounded-xl bg-sky-100 flex items-center justify-center text-sky-600">
                        <i id="modal-icon" data-lucide="info" class="w-6 h-6"></i>
                    </div>
                    <h3 id="modal-title" class="text-lg font-bold text-slate-900">Modal Title</h3>
                </div>
                <button onclick="closeModal()" class="text-slate-400 hover:text-slate-600 p-1 rounded-lg">
                    <i data-lucide="x" class="w-5 h-5"></i>
                </button>
            </div>
            <div id="modal-body" class="text-sm text-slate-600 space-y-3 mb-6">
                Modal Content...
            </div>
            <div class="flex justify-end">
                <button id="modal-primary-btn" onclick="closeModal()" class="bg-slate-900 hover:bg-slate-800 text-white text-sm font-medium px-5 py-2.5 rounded-xl transition">
                    Got it
                </button>
            </div>
        </div>
    </div>

    <!-- Footer -->
    <footer class="bg-slate-900 border-t border-slate-800 text-slate-400 text-xs py-4 px-6 text-center">
        <div class="max-w-7xl mx-auto flex flex-col sm:flex-row justify-between items-center space-y-2 sm:space-y-0">
            <p>© Sec 3 G3 Social Studies Learning Resource — Inquiry: <em>"How does globalisation create different opportunities & challenges for different people?"</em></p>
            <p class="text-slate-500">Singapore Curriculum Aligned</p>
        </div>
    </footer>

    <script>
        const PERSONAS = {
            'mr-tan': {
                id: 'mr-tan',
                name: 'Mr. Tan',
                communityPosition: 'Heritage SME & Local Bakery Sector',
                age: 52,
                role: 'Traditional Bakery Owner',
                location: 'Katong Heritage Precinct',
                avatarUrl: 'https://images.unsplash.com/photo-1556157382-97eda2d62296?auto=format&fit=crop&w=400&q=80',
                avatarBg: 'from-amber-600 via-orange-600 to-amber-700',
                badgeColor: 'bg-amber-100 text-amber-800 border-amber-300',
                icon: 'store',
                tagline: 'Preserving Katong Heritage vs. Facing Global Chains',
                quote: "My family has baked nonya pastries and traditional Hainanese loaves in Katong for 30 years. Global supply chains give us cheap European butter, but global franchises steal our younger crowd.",
                driver: 'Growth of Multinational Corporations (MNCs) & Trade',
                opportunity: 'Access to high-quality imported raw materials at competitive rates.',
                challenge: 'Fierce competition from MNC chains, soaring shop rentals, and labor shortages.',
                scenarios: [
                    {
                        id: 's1',
                        title: 'Scenario 1: Rising Rental & Global Competition in Katong',
                        situation: 'A multinational coffee and bakery chain opens right across your shop along East Coast Road, offering aggressive app discounts and trendy pastries. Meanwhile, your heritage shop rental rises by 15%. What is your primary strategic move?',
                        options: [
                            {
                                text: 'Pivot towards digital delivery platforms (GrabFood/Foodpanda) and highlight authentic Katong heritage baking.',
                                correct: true,
                                score: 10,
                                feedback: 'Excellent adaptation! Embracing digital platforms allows traditional SMEs to tap into younger tech-savvy demographics, though it requires tech re-skilling and commission costs.',
                                ssConcept: 'Adaptation & Digitalization in SMEs'
                            },
                            {
                                text: 'Lower your prices drastically by reducing traditional butter quality.',
                                correct: false,
                                score: 2,
                                feedback: 'Compromising quality erodes your traditional brand strength and customer loyalty without solving long-term cost pressure from massive MNCs.',
                                ssConcept: 'Brand Equity vs Cost Wars'
                            },
                            {
                                text: 'Petition the government to ban foreign bakery franchises in heritage districts.',
                                correct: false,
                                score: 5,
                                feedback: 'Protectionism harms Singapore’s open market standing and consumer choice. The government balances open trade with SME support schemes like SME Go Digital instead.',
                                ssConcept: 'Free Trade vs Protectionism Trade-offs'
                            }
                        ]
                    },
                    {
                        id: 's2',
                        title: 'Scenario 2: Imported Ingredients Sourcing',
                        situation: 'Global supply chain disruptions cause the price of regional flour to spike. However, high-grade European butter and flour imports remain cost-effective due to Singapore\'s open trade tariffs. How do you respond?',
                        options: [
                            {
                                text: 'Leverage Singapore\'s global trade networks to source imported flour while highlighting high quality in your marketing.',
                                correct: true,
                                score: 10,
                                feedback: 'Spot on! Free trade allows businesses to optimize material costs and source high-quality inputs globally.',
                                ssConcept: 'Free Trade & Global Sourcing'
                            },
                            {
                                text: 'Stop baking traditional pastries and switch exclusively to selling imported western breads.',
                                correct: false,
                                score: 4,
                                feedback: 'Abandoning traditional pastries erodes local food heritage and leaves you competing directly on MNC turf.',
                                ssConcept: 'Cultural Preservation vs Commercialization'
                            }
                        ]
                    }
                ],
                debatePrompts: [
                    {
                        question: "In the Townhall, an MNC representative claims: 'Foreign bakery chains elevate Singapore’s food scene and create jobs.' How should Mr. Tan counter effectively?",
                        options: [
                            {
                                text: "'While MNCs bring variety, uncontrolled expansion squeezes out heritage SMEs who cannot compete with global marketing budgets, leading to loss of unique local identity.'",
                                correct: true,
                                explanation: 'Demonstrates a balanced G3 perspective: acknowledging MNC benefits while highlighting the trade-off of cultural homogenization and SME strain.'
                            },
                            {
                                text: "'MNCs are completely bad for Singapore and shouldn't be allowed to operate here.'",
                                correct: false,
                                explanation: 'Too one-sided. Ignores the benefits of foreign direct investment, consumer choice, and employment created by MNCs.'
                            }
                        ]
                    }
                ]
            },
            'elena': {
                id: 'elena',
                name: 'Ms. Elena Rostova',
                communityPosition: 'MNC & Foreign Tech Investment',
                age: 41,
                role: 'Regional Managing Director, Tech Logistics MNC',
                location: 'Changi Business Park',
                avatarUrl: 'https://images.unsplash.com/photo-1573496359142-b8d87734a5a2?auto=format&fit=crop&w=400&q=80',
                avatarBg: 'from-indigo-600 via-blue-700 to-indigo-800',
                badgeColor: 'bg-indigo-100 text-indigo-800 border-indigo-300',
                icon: 'globe',
                tagline: 'Driving High-Tech Investment & Supply Chain Connectivity',
                quote: "Singapore is our premier Asian gateway. We bring foreign direct investment and high-tech software jobs, but global geopolitical tensions keep us on edge.",
                driver: 'Advances in Transportation & Digital Connectivity',
                opportunity: 'Expansive regional market reach, world-class infrastructure, and high FDI inflows.',
                challenge: 'Global economic volatility, supply chain disruptions, and high local operational costs.',
                scenarios: [
                    {
                        id: 's1',
                        title: 'Scenario 1: Automating Warehouse Operations vs Local Jobs',
                        situation: 'To stay competitive against logistics hubs in neighboring countries, your headquarters requires you to implement AI-driven robotic sorting in Changi, which will reduce manual warehouse roles by 30%. What action do you take?',
                        options: [
                            {
                                text: 'Partner with SkillsFuture and government agencies to re-skill impacted local workers for AI monitoring and supervisor roles.',
                                correct: true,
                                score: 10,
                                feedback: 'Outstanding corporate responsibility! Bridging technological advancement with worker upskilling mitigates job disruption.',
                                ssConcept: 'Job Disruption & Re-skilling Policies'
                            },
                            {
                                text: 'Halt all automation to protect existing low-skilled job roles indefinitely.',
                                correct: false,
                                score: 4,
                                feedback: 'Delaying technology adoption weakens the MNC’s global competitiveness and risks relocating operations to cheaper countries.',
                                ssConcept: 'Economic Competitiveness vs Inertia'
                            }
                        ]
                    }
                ],
                debatePrompts: [
                    {
                        question: "A gig worker accuses your MNC: 'You replace human workers with algorithms whenever costs rise!' How do you defend your company's role in Singapore?",
                        options: [
                            {
                                text: "'Automation is essential for global survival, but we actively re-invest in local workforce capability transfer and higher-value tech job creation.'",
                                correct: true,
                                explanation: 'Recognizes economic imperatives while taking accountability for technology transfer and workforce capability upgrading.'
                            },
                            {
                                text: "'Business is business; workers should just adapt on their own without our help.'",
                                correct: false,
                                explanation: 'Lacks social responsibility and fails to reflect sustainable public-private partnership models in Singapore.'
                            }
                        ]
                    }
                ]
            },
            'rahimah': {
                id: 'rahimah',
                name: 'Mdm. Rahimah',
                communityPosition: 'Parenting & Cultural Heritage Community',
                age: 44,
                role: 'Parent of Two Teenagers',
                location: 'Tampines West HDB Township',
                avatarUrl: 'https://images.unsplash.com/photo-1588516903720-8ceb67f9ef84?auto=format&fit=crop&w=400&q=80',
                avatarBg: 'from-rose-600 via-pink-600 to-rose-700',
                badgeColor: 'bg-rose-100 text-rose-800 border-rose-300',
                icon: 'heart',
                tagline: 'Global Opportunities vs. Cultural Dilution & Future Stress',
                quote: "My teenagers in Tampines have access to global online courses and international friends, but Western pop media dominates their daily life. Will they lose their cultural roots?",
                driver: 'Cultural Hybridity & Mass Media Connectivity',
                opportunity: 'Access to world-class educational tools, global exposure, and diverse lifestyles.',
                challenge: 'Anxiety over children’s future job security and the dilution of traditional cultural values.',
                scenarios: [
                    {
                        id: 's1',
                        title: 'Scenario 1: Managing Media Exposure & Cultural Values',
                        situation: 'Your teenagers spend hours on global social platforms (TikTok/YouTube), adopting Western slang, global consumer habits, and neglecting ethnic language practice. How do you address this at home?',
                        options: [
                            {
                                text: 'Encourage active participation in local community cultural activities and family discussions to foster balanced bicultural confidence.',
                                correct: true,
                                score: 10,
                                feedback: 'Great approach! Fostering cultural rootedness alongside global open-mindedness creates resilient identities.',
                                ssConcept: 'Rooted Cosmopolitan Identity'
                            },
                            {
                                text: 'Ban all access to international internet media and mobile phones.',
                                correct: false,
                                score: 3,
                                feedback: 'Total restrictions are impractical in a digital economy and deprive youth of vital digital literacy skills.',
                                ssConcept: 'Digital Literacy vs Isolation'
                            }
                        ]
                    }
                ],
                debatePrompts: [
                    {
                        question: "How can Mdm. Rahimah explain why cultural dilution is a genuine societal concern to young students?",
                        options: [
                            {
                                text: "'When foreign trends replace local traditions, intergenerational bonds weaken and our unique multicultural heritage risks becoming homogenous.'",
                                correct: true,
                                explanation: 'Articulates how cultural homogenization affects social cohesion and heritage preservation.'
                            },
                            {
                                text: "'Foreign culture is bad and corrupts our youth.'",
                                correct: false,
                                explanation: 'Simplistic and dismissive of the positive benefits of cross-cultural learning and exposure.'
                            }
                        ]
                    }
                ]
            },
            'ethan': {
                id: 'ethan',
                name: 'Ethan',
                communityPosition: 'Youth & Technological Talent Workforce',
                age: 19,
                role: 'Software Engineering Undergraduate',
                location: 'NUS Kent Ridge Campus',
                avatarUrl: 'https://images.unsplash.com/photo-1534528741775-53994a69daeb?auto=format&fit=crop&w=400&q=80',
                avatarBg: 'from-cyan-600 via-blue-600 to-cyan-700',
                badgeColor: 'bg-cyan-100 text-cyan-800 border-cyan-300',
                icon: 'laptop',
                tagline: 'Global Career Horizons vs. Intense Talent Competition',
                quote: "From my desk at Kent Ridge, I can work with developers in Silicon Valley or Seoul. But I'm competing with top international talent for local tech roles in Singapore.",
                driver: 'Global Mobility of Talent & Technological Speed',
                opportunity: 'Unprecedented access to global remote work, global knowledge exchange, and overseas internships.',
                challenge: 'High competition from international talent and rapid skill obsolescence due to AI.',
                scenarios: [
                    {
                        id: 's1',
                        title: 'Scenario 1: Navigating Career Readiness in an AI Era',
                        situation: 'A new foreign startup sets up in Singapore, bringing top AI talent from abroad. Basic coding tasks are now automated. How do you secure your competitive edge?',
                        options: [
                            {
                                text: 'Focus on developing cross-disciplinary skills like AI system architecture, emotional intelligence, and complex problem solving.',
                                correct: true,
                                score: 10,
                                feedback: 'Spot on! Moving up the value chain ensures long-term employability in a rapidly evolving global talent market.',
                                ssConcept: 'Moving Up the Value Chain'
                            },
                            {
                                text: 'Demand the government guarantee local graduate hires regardless of skill level.',
                                correct: false,
                                score: 4,
                                feedback: 'Without continuous skill upgrading, artificially protecting jobs reduces overall national economic innovation.',
                                ssConcept: 'Economic Innovation & Talent Mobility'
                            }
                        ]
                    }
                ],
                debatePrompts: [
                    {
                        question: "In a discussion on foreign talent policies, how should Ethan balance local worker protection with Singapore’s global hub ambitions?",
                        options: [
                            {
                                text: "'Singapore must remain open to global top talent to stay innovative, but needs fair employment guidelines (like the COMPASS framework) so local grads get fair opportunities.'",
                                correct: true,
                                explanation: 'Reflects Singapore’s balanced policy stance: staying open while safeguarding fair local career progression.'
                            },
                            {
                                text: "'Close the borders to foreign workers completely so local graduates get instant jobs.'",
                                correct: false,
                                explanation: 'Ignoring global talent inflows risks driving global firms to establish headquarters elsewhere.'
                            }
                        ]
                    }
                ]
            },
            'krishnan': {
                id: 'krishnan',
                name: 'Uncle Krishnan',
                communityPosition: 'Senior Citizens & Pioneer Generation',
                age: 68,
                role: 'Retiree & Community Volunteer',
                location: 'Toa Payoh Central',
                avatarUrl: 'https://images.unsplash.com/photo-1506794778202-cad84cf45f1d?auto=format&fit=crop&w=400&q=80',
                avatarBg: 'from-emerald-600 via-teal-600 to-emerald-700',
                badgeColor: 'bg-emerald-100 text-emerald-800 border-emerald-300',
                icon: 'users',
                tagline: 'Advanced Infrastructure vs. Digital Divide & Fast Pace',
                quote: "Singapore’s healthcare and MRT transport are world-class. But when every hawker stall in Toa Payoh requires a smartphone app or QR code, many elderly feel left behind.",
                driver: 'Rapid Technological Influx & Digitalization',
                opportunity: 'High-quality public goods, superior healthcare, and safe urban environments.',
                challenge: 'Digital divide, feelings of alienation, and risk of falling victim to online scams.',
                scenarios: [
                    {
                        id: 's1',
                        title: 'Scenario 1: Cashless Hawker Centers & Digital Exclusion',
                        situation: 'Your neighborhood hawker center in Toa Payoh transitions to 100% cashless SGQR payments. Many older residents struggle to order meals and fear digital fraud. How do you respond as a community volunteer?',
                        options: [
                            {
                                text: 'Organize peer-led Digital Ambassador workshops while advocating for hawkers to retain cash payment options for seniors.',
                                correct: true,
                                score: 10,
                                feedback: 'Excellent community initiative! Combines digital empowerment with inclusive policy safeguards for vulnerable groups.',
                                ssConcept: 'Digital Inclusion & Inclusivity'
                            },
                            {
                                text: 'Advocate shutting down all digital payment technology to keep things simple.',
                                correct: false,
                                score: 3,
                                feedback: 'Reverting technological progress hinders productivity and efficiency across the wider economy.',
                                ssConcept: 'Digital Productivity vs Accessibility'
                            }
                        ]
                    }
                ],
                debatePrompts: [
                    {
                        question: "How can Uncle Krishnan best illustrate the concept of the 'Digital Divide' to policy makers?",
                        options: [
                            {
                                text: "'While digital tech brings efficiency to youth, seniors without digital literacy face barriers to basic daily services, creating social inequality.'",
                                correct: true,
                                explanation: 'Accurately defines how uneven technological adoption leads to social disadvantage for non-tech-savvy demographics.'
                            },
                            {
                                text: "'Old people just don't like technology and don't care about progress.'",
                                correct: false,
                                explanation: 'Overly stereotypical and ignores structural barriers like physical constraints, cognitive hurdles, and security fears.'
                            }
                        ]
                    }
                ]
            },
            'ah-hock': {
                id: 'ah-hock',
                name: 'Ah Hock',
                communityPosition: 'Gig Economy & Platform Workers',
                age: 38,
                role: 'Delivery Driver & Ex-Factory Worker',
                location: 'Jurong West Industrial & Residential',
                avatarUrl: 'https://images.unsplash.com/photo-1542909168-82c3e7fdca5c?auto=format&fit=crop&w=400&q=80',
                avatarBg: 'from-violet-600 via-purple-600 to-violet-700',
                badgeColor: 'bg-violet-100 text-violet-800 border-violet-300',
                icon: 'bike',
                tagline: 'Flexible Platform Work vs. Income Insecurity & Displacement',
                quote: "When my Jurong factory job was automated, food delivery apps gave me immediate income flexibility. But I lack CPF retirement support and medical safety nets.",
                driver: 'Platform Economy & Technological Displacement',
                opportunity: 'Flexible, immediate low-barrier employment enabled by global tech apps.',
                challenge: 'Income volatility, lack of employment benefits, and vulnerability to wage stagnation.',
                scenarios: [
                    {
                        id: 's1',
                        title: 'Scenario 1: Upskilling Dilemma - Time vs Daily Earnings',
                        situation: 'The government offers heavily subsidized drone delivery & logistics operator training in Jurong. However, attending the 3-week course means losing daily delivery income. What choice do you make?',
                        options: [
                            {
                                text: 'Apply for Career Conversion schemes that provide training allowances to offset lost income while learning long-term skills.',
                                correct: true,
                                score: 10,
                                feedback: 'Smart decision! Targeted government support allowances help low-wage workers overcome short-term income trade-offs to achieve long-term employability.',
                                ssConcept: 'Government Support & Workforce Resilience'
                            },
                            {
                                text: 'Skip training entirely and work 16 hours a day on deliveries to maximize immediate cash flow.',
                                correct: false,
                                score: 4,
                                feedback: 'Provides short-term cash but risks burnout and leaves you vulnerable as platform algorithm wages fluctuate or automate.',
                                ssConcept: 'Short-term Gain vs Long-term Sustainability'
                            }
                        ]
                    }
                ],
                debatePrompts: [
                    {
                        question: "In a debate on gig worker welfare, what is Ah Hock’s strongest argument for regulatory reforms?",
                        options: [
                            {
                                text: "'Gig work is a byproduct of globalisation. Platform workers need baseline CPF contributions and injury protection to ensure long-term social safety nets.'",
                                correct: true,
                                explanation: 'Highlights the trade-offs of flexible platform economies and the need for evolving policy protection (e.g., Singapore\'s Platform Workers Act).'
                            },
                            {
                                text: "'Delivery platforms should be banned so everyone gets factory jobs back.'",
                                correct: false,
                                explanation: 'Unrealistic in a modern digital economy where e-commerce is a key economic driver.'
                            }
                        ]
                    }
                ]
            }
        };

        const GLOSSARY_TERMS = [
            {
                term: "Globalisation",
                definition: "The growing interconnectedness and interdependence of countries through global trade, technology, cultural exchanges, and movement of people.",
                example: "Order food from an American fast-food chain in Katong using a Chinese-made smartphone powered by European software."
            },
            {
                term: "Multinational Corporations (MNCs)",
                definition: "Large businesses that operate and have facilities in multiple countries outside their home nation.",
                example: "Tech and logistics hubs operating in Changi Business Park driving foreign direct investment."
            },
            {
                term: "Foreign Direct Investment (FDI)",
                definition: "Investments made by foreign companies or individuals into domestic business operations and infrastructure.",
                example: "Overseas tech firms investing in data centers and AI research labs in Singapore."
            },
            {
                term: "Trade-offs",
                definition: "A situational decision where giving up one benefit or quality is necessary to gain another.",
                example: "Welcoming foreign talent boosts economic competitiveness (benefit) but increases local job competition (trade-off)."
            },
            {
                term: "Cultural Dilution / Homogenisation",
                definition: "The process where unique local traditions and cultural practices are reduced or replaced by dominant global cultures.",
                example: "Younger generations adopting global media trends over local heritage languages or customs."
            },
            {
                term: "Digital Divide",
                definition: "The gap between demographics who have easy access to digital technology/literacy and those who do not.",
                example: "Seniors struggling to navigate cashless mobile payment apps at hawker centers."
            },
            {
                term: "Job Disruption & Re-skilling",
                definition: "The elimination or transformation of job roles due to technological changes, requiring workers to learn new skills.",
                example: "Factory technicians upskilling into automated robotics operators or logistics supervisors."
            }
        ];

        let state = {
            currentView: 'hub', // 'hub', 'persona-detail', 'scenario', 'debate', 'glossary', 'synthesis'
            selectedPersonaId: null,
            personaProgress: {
                'mr-tan': { scenarioDone: false, debateDone: false, score: 0 },
                'elena': { scenarioDone: false, debateDone: false, score: 0 },
                'rahimah': { scenarioDone: false, debateDone: false, score: 0 },
                'ethan': { scenarioDone: false, debateDone: false, score: 0 },
                'krishnan': { scenarioDone: false, debateDone: false, score: 0 },
                'ah-hock': { scenarioDone: false, debateDone: false, score: 0 }
            },
            activeScenarioIdx: 0,
            activeDebateIdx: 0,
            synthesisPolicies: {
                fdiOpenness: 70, // 0-100
                digitalSpeed: 80, // 0-100
                heritageProtection: 60 // 0-100
            }
        };

        // Initialize App
        window.addEventListener('DOMContentLoaded', () => {
            loadProgressFromStorage();
            navigateTo('hub');
        });

        function saveProgressToStorage() {
            try {
                localStorage.setItem('sec3_ss_globalisation_progress', JSON.stringify(state.personaProgress));
            } catch (e) { console.error('Local storage blocked', e); }
        }

        function loadProgressFromStorage() {
            try {
                const saved = localStorage.getItem('sec3_ss_globalisation_progress');
                if (saved) {
                    state.personaProgress = Object.assign(state.personaProgress, JSON.parse(saved));
                }
            } catch (e) { console.error(e); }
        }

        function navigateTo(viewName, personaId = null) {
            state.currentView = viewName;
            if (personaId) state.selectedPersonaId = personaId;
            
            const container = document.getElementById('main-container');
            container.innerHTML = ''; // Clear container

            switch (viewName) {
                case 'hub':
                    renderHubView(container);
                    break;
                case 'persona-detail':
                    renderPersonaDetailView(container);
                    break;
                case 'scenario':
                    renderScenarioView(container);
                    break;
                case 'debate':
                    renderDebateView(container);
                    break;
                case 'glossary':
                    renderGlossaryView(container);
                    break;
                case 'synthesis':
                    renderSynthesisView(container);
                    break;
                default:
                    renderHubView(container);
            }

            // Re-initialize Lucide Icons
            if (window.lucide) {
                lucide.createIcons();
            }
            window.scrollTo({ top: 0, behavior: 'smooth' });
        }

        function renderHubView(container) {
            const html = `
                <!-- Hub Header Banner -->
                <div class="bg-gradient-to-r from-slate-900 via-sky-950 to-slate-900 rounded-3xl p-6 sm:p-10 text-white shadow-xl mb-8 relative overflow-hidden border border-slate-800">
                    <div class="absolute -right-10 -bottom-10 opacity-10 pointer-events-none">
                        <i data-lucide="globe" class="w-96 h-96 text-white"></i>
                    </div>
                    <div class="max-w-3xl relative z-10">
                        <span class="inline-flex items-center space-x-2 bg-sky-500/20 text-sky-300 border border-sky-400/30 px-3 py-1 rounded-full text-xs font-semibold uppercase tracking-wider mb-4">
                            <i data-lucide="sparkles" class="w-3.5 h-3.5"></i>
                            <span>Sec 3 G3 Social Studies Simulation</span>
                        </span>
                        <h2 class="text-2xl sm:text-4xl font-extrabold tracking-tight mb-3">Choose Your Stakeholder Persona</h2>
                        <p class="text-slate-300 text-sm sm:text-base leading-relaxed mb-6">
                            Pick any stakeholder to explore their story and scenarios directly! You can choose any scenario to play at your own pace, or test national policy trade-offs in the Townhall.
                        </p>
                        <div class="flex flex-wrap gap-3">
                            <button onclick="navigateTo('synthesis')" class="bg-sky-500 hover:bg-sky-600 text-white font-semibold text-sm px-5 py-2.5 rounded-xl transition shadow-lg flex items-center space-x-2">
                                <i data-lucide="scale" class="w-4 h-4"></i>
                                <span>Townhall Policy Simulation</span>
                            </button>
                            <button onclick="navigateTo('glossary')" class="bg-slate-800 hover:bg-slate-700 text-slate-200 border border-slate-700 font-semibold text-sm px-4 py-2.5 rounded-xl transition flex items-center space-x-2">
                                <i data-lucide="book-open" class="w-4 h-4"></i>
                                <span>Study Key SS Terms</span>
                            </button>
                        </div>
                    </div>
                </div>

                <!-- Grid of 6 Stakeholders -->
                <div class="mb-6 flex justify-between items-center">
                    <h3 class="text-xl font-bold text-slate-900 flex items-center space-x-2">
                        <i data-lucide="users" class="w-5 h-5 text-sky-600"></i>
                        <span>Select a Stakeholder Persona</span>
                    </h3>
                    <span class="text-xs text-slate-500">Pick any persona or scenario directly</span>
                </div>

                <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6 mb-12">
                    ${Object.keys(PERSONAS).map(id => {
                        const p = PERSONAS[id];
                        const prog = state.personaProgress[id] || { scenarioDone: false, debateDone: false, score: 0 };
                        const isMastered = prog.scenarioDone && prog.debateDone;

                        return `
                            <div class="bg-white rounded-2xl border border-slate-200 hover:border-sky-400 shadow-sm hover:shadow-xl transition-all duration-300 flex flex-col overflow-hidden group">
                                <!-- Card Top Banner with Face Image -->
                                <div class="bg-gradient-to-r ${p.avatarBg} p-5 text-white relative flex items-center space-x-4">
                                    <div class="relative shrink-0">
                                        <img src="${p.avatarUrl}" alt="${p.name}" class="w-16 h-16 rounded-2xl object-cover ring-2 ring-white/50 shadow-md" onerror="this.onerror=null; this.src='https://placehold.co/150x150/0284c7/ffffff?text=${encodeURIComponent(p.name)}';">
                                        <div class="absolute -bottom-1 -right-1 w-6 h-6 rounded-lg bg-slate-900 text-amber-300 flex items-center justify-center text-xs shadow">
                                            <i data-lucide="${p.icon}" class="w-3.5 h-3.5"></i>
                                        </div>
                                    </div>
                                    <div class="flex-1 min-w-0">
                                        <div class="flex items-center justify-between">
                                            <h4 class="text-lg font-bold truncate">${p.name}</h4>
                                            ${isMastered ? `
                                                <span class="bg-amber-400 text-slate-900 text-[10px] font-bold px-2 py-0.5 rounded-full flex items-center space-x-1 shrink-0">
                                                    <i data-lucide="check" class="w-3 h-3"></i>
                                                    <span>Done</span>
                                                </span>
                                            ` : ''}
                                        </div>
                                        <!-- Community Position Badge beside/under name -->
                                        <span class="inline-block bg-black/30 backdrop-blur-sm text-sky-200 text-[11px] font-semibold px-2 py-0.5 rounded-md mt-1 border border-white/10 truncate max-w-full" title="${p.communityPosition}">
                                            ${p.communityPosition}
                                        </span>
                                        <p class="text-[11px] text-white/80 font-medium truncate mt-0.5">${p.role} • ${p.location}</p>
                                    </div>
                                </div>

                                <!-- Card Body -->
                                <div class="p-5 flex-1 flex flex-col justify-between space-y-4">
                                    <div>
                                        <div class="inline-block px-2.5 py-1 rounded-md text-xs font-semibold mb-3 ${p.badgeColor}">
                                            ${p.driver}
                                        </div>
                                        <p class="text-xs text-slate-600 italic leading-relaxed mb-3">
                                            "${p.quote}"
                                        </p>
                                        <div class="space-y-2 text-xs">
                                            <div class="flex items-start space-x-2 text-emerald-700 bg-emerald-50 p-2 rounded-lg">
                                                <i data-lucide="arrow-up-right" class="w-4 h-4 shrink-0 mt-0.5"></i>
                                                <span><strong>Opportunity:</strong> ${p.opportunity}</span>
                                            </div>
                                            <div class="flex items-start space-x-2 text-rose-700 bg-rose-50 p-2 rounded-lg">
                                                <i data-lucide="arrow-down-right" class="w-4 h-4 shrink-0 mt-0.5"></i>
                                                <span><strong>Challenge:</strong> ${p.challenge}</span>
                                            </div>
                                        </div>
                                    </div>

                                    <!-- Direct Scenario Selection Actions -->
                                    <div class="space-y-2 pt-2 border-t border-slate-100">
                                        <span class="text-[11px] font-bold text-slate-400 uppercase tracking-wider block">Pick a Scenario directly:</span>
                                        <div class="grid grid-cols-1 gap-1.5">
                                            ${p.scenarios.map((s, idx) => `
                                                <button onclick="state.activeScenarioIdx=${idx}; navigateTo('scenario', '${p.id}')" class="w-full text-left bg-slate-50 hover:bg-sky-50 text-slate-700 hover:text-sky-900 border border-slate-200 hover:border-sky-300 text-xs py-2 px-3 rounded-xl transition flex items-center justify-between font-medium group">
                                                    <span class="truncate pr-2">${s.title}</span>
                                                    <i data-lucide="play" class="w-3.5 h-3.5 text-sky-600 shrink-0 group-hover:translate-x-0.5 transition-transform"></i>
                                                </button>
                                            `).join('')}
                                        </div>
                                        <button onclick="navigateTo('persona-detail', '${p.id}')" class="w-full bg-slate-900 hover:bg-sky-600 text-white text-xs font-bold py-2.5 rounded-xl transition flex items-center justify-center space-x-2 mt-2">
                                            <span>View Full Profile & Debate</span>
                                            <i data-lucide="arrow-right" class="w-3.5 h-3.5"></i>
                                        </button>
                                    </div>
                                </div>
                            </div>
                        `;
                    }).join('')}
                </div>
            `;
            container.innerHTML = html;
        }

        function renderPersonaDetailView(container) {
            const p = PERSONAS[state.selectedPersonaId];
            if (!p) return navigateTo('hub');

            const prog = state.personaProgress[p.id] || { scenarioDone: false, debateDone: false, score: 0 };

            const html = `
                <div class="mb-6">
                    <button onclick="navigateTo('hub')" class="inline-flex items-center space-x-1.5 text-xs font-semibold text-slate-500 hover:text-slate-800 transition">
                        <i data-lucide="chevron-left" class="w-4 h-4"></i>
                        <span>Back to Stakeholder Hub</span>
                    </button>
                </div>

                <!-- Stakeholder Profile Banner -->
                <div class="bg-white rounded-3xl border border-slate-200 shadow-lg overflow-hidden mb-8">
                    <div class="bg-gradient-to-r ${p.avatarBg} p-6 sm:p-8 text-white flex flex-col sm:flex-row justify-between items-start sm:items-center gap-6">
                        <div class="flex items-center space-x-5">
                            <img src="${p.avatarUrl}" alt="${p.name}" class="w-20 h-20 sm:w-24 sm:h-24 rounded-2xl object-cover ring-4 ring-white/30 shadow-xl" onerror="this.onerror=null; this.src='https://placehold.co/150x150/0284c7/ffffff?text=${encodeURIComponent(p.name)}';">
                            <div>
                                <div class="flex flex-wrap items-center gap-2 mb-1">
                                    <h2 class="text-2xl sm:text-3xl font-extrabold">${p.name}</h2>
                                    <span class="bg-white/20 backdrop-blur-md text-amber-200 border border-white/20 text-xs font-bold px-3 py-1 rounded-full shadow-sm">
                                        Community Position: ${p.communityPosition}
                                    </span>
                                </div>
                                <p class="text-sm text-white/90 font-medium">${p.role} • ${p.location} (Age ${p.age})</p>
                                <p class="text-xs text-white/80 italic mt-1">"${p.tagline}"</p>
                            </div>
                        </div>
                        <div class="bg-white/10 backdrop-blur-md border border-white/20 px-5 py-3 rounded-2xl shrink-0">
                            <span class="text-xs text-white/70 block">Stakeholder Score</span>
                            <span class="text-2xl font-black text-amber-300">${prog.score} pts</span>
                        </div>
                    </div>

                    <!-- Profile Overview Grid -->
                    <div class="p-6 sm:p-8 grid grid-cols-1 md:grid-cols-3 gap-6 bg-slate-50/50">
                        <div class="bg-white p-4 rounded-xl border border-slate-200 shadow-sm">
                            <span class="text-xs font-bold text-slate-400 uppercase tracking-wider block mb-1">Key Driver</span>
                            <p class="text-sm font-semibold text-slate-800">${p.driver}</p>
                        </div>
                        <div class="bg-white p-4 rounded-xl border border-slate-200 shadow-sm">
                            <span class="text-xs font-bold text-emerald-600 uppercase tracking-wider block mb-1">Primary Opportunity</span>
                            <p class="text-xs text-slate-700 leading-relaxed">${p.opportunity}</p>
                        </div>
                        <div class="bg-white p-4 rounded-xl border border-slate-200 shadow-sm">
                            <span class="text-xs font-bold text-rose-600 uppercase tracking-wider block mb-1">Primary Challenge</span>
                            <p class="text-xs text-slate-700 leading-relaxed">${p.challenge}</p>
                        </div>
                    </div>
                </div>

                <!-- Game Mode Selection Cards -->
                <h3 class="text-lg font-bold text-slate-900 mb-4 flex items-center space-x-2">
                    <i data-lucide="gamepad-2" class="w-5 h-5 text-sky-600"></i>
                    <span>Select Any Activity for ${p.name} (${p.communityPosition})</span>
                </h3>

                <div class="grid grid-cols-1 md:grid-cols-2 gap-6 mb-12">
                    <!-- Direct Scenario List -->
                    <div class="bg-white rounded-2xl border ${prog.scenarioDone ? 'border-emerald-300 bg-emerald-50/20' : 'border-slate-200'} p-6 shadow-sm hover:shadow-md transition flex flex-col justify-between space-y-4">
                        <div>
                            <div class="flex justify-between items-start mb-3">
                                <div class="w-10 h-10 rounded-xl bg-sky-100 text-sky-700 flex items-center justify-center font-bold">
                                    <i data-lucide="compass" class="w-5 h-5"></i>
                                </div>
                                ${prog.scenarioDone ? `
                                    <span class="bg-emerald-100 text-emerald-800 text-xs font-bold px-2.5 py-1 rounded-full flex items-center space-x-1">
                                        <i data-lucide="check" class="w-3.5 h-3.5"></i>
                                        <span>Completed</span>
                                    </span>
                                ` : `
                                    <span class="bg-slate-100 text-slate-600 text-xs font-medium px-2.5 py-1 rounded-full">${p.scenarios.length} Scenario(s) Available</span>
                                `}
                            </div>
                            <h4 class="text-lg font-bold text-slate-900 mb-1">Scenario Decision Dilemmas</h4>
                            <p class="text-xs text-slate-600 leading-relaxed mb-4">
                                Jump directly into any scenario faced by ${p.name}. Make decisions balancing economic, social, and personal trade-offs.
                            </p>

                            <div class="space-y-2">
                                ${p.scenarios.map((s, idx) => `
                                    <button onclick="state.activeScenarioIdx=${idx}; navigateTo('scenario', '${p.id}')" class="w-full text-left bg-sky-50/80 hover:bg-sky-100 text-sky-900 border border-sky-200 text-xs font-semibold py-2.5 px-3.5 rounded-xl transition flex items-center justify-between group">
                                        <span>${s.title}</span>
                                        <i data-lucide="play" class="w-4 h-4 text-sky-600 group-hover:translate-x-1 transition-transform"></i>
                                    </button>
                                `).join('')}
                            </div>
                        </div>
                    </div>

                    <!-- Townhall Debate Response -->
                    <div class="bg-white rounded-2xl border ${prog.debateDone ? 'border-emerald-300 bg-emerald-50/20' : 'border-slate-200'} p-6 shadow-sm hover:shadow-md transition flex flex-col justify-between space-y-4">
                        <div>
                            <div class="flex justify-between items-start mb-3">
                                <div class="w-10 h-10 rounded-xl bg-indigo-100 text-indigo-700 flex items-center justify-center font-bold">
                                    <i data-lucide="message-square" class="w-5 h-5"></i>
                                </div>
                                ${prog.debateDone ? `
                                    <span class="bg-emerald-100 text-emerald-800 text-xs font-bold px-2.5 py-1 rounded-full flex items-center space-x-1">
                                        <i data-lucide="check" class="w-3.5 h-3.5"></i>
                                        <span>Completed</span>
                                    </span>
                                ` : `
                                    <span class="bg-slate-100 text-slate-600 text-xs font-medium px-2.5 py-1 rounded-full">Debate Mode</span>
                                `}
                            </div>
                            <h4 class="text-lg font-bold text-slate-900 mb-1">Townhall Debate Defense</h4>
                            <p class="text-xs text-slate-600 leading-relaxed mb-4">
                                Stand up in the simulated National Townhall. Formulate well-reasoned arguments representing ${p.name}'s community stance.
                            </p>
                        </div>
                        <button onclick="state.activeDebateIdx=0; navigateTo('debate', '${p.id}')" class="bg-indigo-600 hover:bg-indigo-700 text-white text-xs font-bold py-3 px-4 rounded-xl transition flex items-center justify-center space-x-2">
                            <span>Start Townhall Speech</span>
                            <i data-lucide="mic" class="w-4 h-4"></i>
                        </button>
                    </div>
                </div>
            `;
            container.innerHTML = html;
        }

        function renderScenarioView(container) {
            const p = PERSONAS[state.selectedPersonaId];
            if (!p) return navigateTo('hub');

            const scenario = p.scenarios[state.activeScenarioIdx];
            if (!scenario) {
                // Scenarios complete!
                state.personaProgress[p.id].scenarioDone = true;
                saveProgressToStorage();

                confetti({ particleCount: 60, spread: 60, origin: { y: 0.6 } });

                showModal(
                    "Scenario Complete!",
                    `You finished this scenario for <strong>${p.name}</strong> (${p.communityPosition})! Feel free to pick another scenario or move on to the debate.`,
                    "message-square",
                    () => navigateTo('persona-detail', p.id)
                );
                return;
            }

            const html = `
                <div class="mb-4 flex flex-wrap justify-between items-center gap-2">
                    <button onclick="navigateTo('persona-detail', '${p.id}')" class="inline-flex items-center space-x-1 text-xs font-semibold text-slate-500 hover:text-slate-800">
                        <i data-lucide="chevron-left" class="w-4 h-4"></i>
                        <span>Back to ${p.name}'s Profile</span>
                    </button>
                    
                    <!-- Scenario Quick Selector Switcher -->
                    <div class="flex items-center space-x-1 bg-slate-200/70 p-1 rounded-xl">
                        ${p.scenarios.map((s, idx) => `
                            <button onclick="state.activeScenarioIdx=${idx}; navigateTo('scenario', '${p.id}')" class="px-3 py-1 rounded-lg text-xs font-bold transition ${state.activeScenarioIdx === idx ? 'bg-white text-sky-700 shadow-sm' : 'text-slate-600 hover:text-slate-900'}">
                                Scenario ${idx + 1}
                            </button>
                        `).join('')}
                    </div>
                </div>

                <!-- Scenario Card -->
                <div class="bg-white rounded-3xl border border-slate-200 shadow-xl overflow-hidden max-w-3xl mx-auto">
                    <!-- Top Bar with Avatar & Position -->
                    <div class="bg-gradient-to-r ${p.avatarBg} p-6 text-white flex items-center justify-between">
                        <div class="flex items-center space-x-4">
                            <img src="${p.avatarUrl}" alt="${p.name}" class="w-12 h-12 rounded-xl object-cover ring-2 ring-white/50 shadow" onerror="this.onerror=null; this.src='https://placehold.co/150x150/0284c7/ffffff?text=${encodeURIComponent(p.name)}';">
                            <div>
                                <div class="flex items-center space-x-2">
                                    <h3 class="font-bold text-lg leading-tight">${p.name}</h3>
                                    <span class="text-[11px] bg-white/20 text-sky-100 px-2.5 py-0.5 rounded-full font-semibold">
                                        ${p.communityPosition}
                                    </span>
                                </div>
                                <p class="text-xs text-white/80">${scenario.title}</p>
                            </div>
                        </div>
                    </div>

                    <!-- Situation Description -->
                    <div class="p-6 sm:p-8 space-y-6">
                        <div class="bg-slate-50 p-5 rounded-2xl border border-slate-200 text-sm text-slate-800 leading-relaxed font-medium">
                            "${scenario.situation}"
                        </div>

                        <h4 class="text-xs font-bold uppercase tracking-wider text-slate-400">What action should ${p.name} take?</h4>

                        <!-- Options List -->
                        <div class="space-y-3" id="scenario-options-container">
                            ${scenario.options.map((opt, idx) => `
                                <button onclick="selectScenarioOption(${idx})" class="w-full text-left p-4 rounded-xl border border-slate-200 hover:border-sky-500 hover:bg-sky-50/50 transition-all flex items-start space-x-3 group">
                                    <div class="w-6 h-6 rounded-full bg-slate-100 group-hover:bg-sky-500 group-hover:text-white text-slate-600 font-bold text-xs flex items-center justify-center shrink-0 mt-0.5 transition">
                                        ${String.fromCharCode(65 + idx)}
                                    </div>
                                    <span class="text-xs sm:text-sm font-medium text-slate-700 group-hover:text-slate-900">${opt.text}</span>
                                </button>
                            `).join('')}
                        </div>

                        <!-- Feedback Area (Hidden initially) -->
                        <div id="scenario-feedback" class="hidden p-5 rounded-2xl border space-y-3">
                            <!-- Feedback injected dynamically -->
                        </div>
                    </div>
                </div>
            `;
            container.innerHTML = html;
        }

        function selectScenarioOption(optIdx) {
            const p = PERSONAS[state.selectedPersonaId];
            const scenario = p.scenarios[state.activeScenarioIdx];
            const selectedOpt = scenario.options[optIdx];

            // Disable all option buttons
            const container = document.getElementById('scenario-options-container');
            const btns = container.querySelectorAll('button');
            btns.forEach((btn, idx) => {
                btn.disabled = true;
                if (idx === optIdx) {
                    btn.classList.add(selectedOpt.correct ? 'border-emerald-500' : 'border-rose-500', 'bg-slate-50');
                } else {
                    btn.classList.add('opacity-50');
                }
            });

            // Update Score
            if (selectedOpt.correct) {
                state.personaProgress[p.id].score += selectedOpt.score;
                saveProgressToStorage();
            }

            // Display Feedback Box
            const feedbackBox = document.getElementById('scenario-feedback');
            feedbackBox.classList.remove('hidden');
            if (selectedOpt.correct) {
                feedbackBox.className = "p-5 rounded-2xl border border-emerald-300 bg-emerald-50/80 text-emerald-900 space-y-3";
                feedbackBox.innerHTML = `
                    <div class="flex items-center space-x-2 font-bold text-sm text-emerald-800">
                        <i data-lucide="check-circle" class="w-5 h-5 text-emerald-600"></i>
                        <span>Strong Decision (+${selectedOpt.score} pts)</span>
                    </div>
                    <p class="text-xs leading-relaxed">${selectedOpt.feedback}</p>
                    <div class="inline-block bg-white/80 border border-emerald-200 px-3 py-1 rounded-lg text-xs font-semibold text-emerald-800">
                        SS Concept: ${selectedOpt.ssConcept}
                    </div>
                    <div class="pt-3 flex justify-end">
                        <button onclick="nextScenario()" class="bg-emerald-700 hover:bg-emerald-800 text-white font-semibold text-xs px-5 py-2.5 rounded-xl transition flex items-center space-x-1.5">
                            <span>Continue</span>
                            <i data-lucide="arrow-right" class="w-4 h-4"></i>
                        </button>
                    </div>
                `;
            } else {
                feedbackBox.className = "p-5 rounded-2xl border border-amber-300 bg-amber-50/80 text-amber-900 space-y-3";
                feedbackBox.innerHTML = `
                    <div class="flex items-center space-x-2 font-bold text-sm text-amber-800">
                        <i data-lucide="alert-triangle" class="w-5 h-5 text-amber-600"></i>
                        <span>Suboptimal Choice (+${selectedOpt.score} pts)</span>
                    </div>
                    <p class="text-xs leading-relaxed">${selectedOpt.feedback}</p>
                    <div class="inline-block bg-white/80 border border-amber-200 px-3 py-1 rounded-lg text-xs font-semibold text-amber-800">
                        SS Concept: ${selectedOpt.ssConcept}
                    </div>
                    <div class="pt-3 flex justify-end">
                        <button onclick="nextScenario()" class="bg-amber-700 hover:bg-amber-800 text-white font-semibold text-xs px-5 py-2.5 rounded-xl transition flex items-center space-x-1.5">
                            <span>Continue</span>
                            <i data-lucide="arrow-right" class="w-4 h-4"></i>
                        </button>
                    </div>
                `;
            }
            if (window.lucide) lucide.createIcons();
        }

        function nextScenario() {
            state.activeScenarioIdx++;
            renderScenarioView(document.getElementById('main-container'));
        }

        function renderDebateView(container) {
            const p = PERSONAS[state.selectedPersonaId];
            if (!p) return navigateTo('hub');

            const prompt = p.debatePrompts[state.activeDebateIdx];
            if (!prompt) {
                // Debate completed
                state.personaProgress[p.id].debateDone = true;
                saveProgressToStorage();

                confetti({ particleCount: 70, spread: 80, origin: { y: 0.6 } });

                showModal(
                    "Townhall Speech Delivered!",
                    `Congratulations! You represented <strong>${p.name}</strong> (${p.communityPosition}) in the National Townhall.`,
                    "award",
                    () => navigateTo('persona-detail', p.id)
                );
                return;
            }

            const html = `
                <div class="mb-4 flex justify-between items-center">
                    <button onclick="navigateTo('persona-detail', '${p.id}')" class="inline-flex items-center space-x-1 text-xs font-semibold text-slate-500 hover:text-slate-800">
                        <i data-lucide="chevron-left" class="w-4 h-4"></i>
                        <span>Exit Townhall</span>
                    </button>
                    <span class="text-xs font-bold text-indigo-600 bg-indigo-50 px-3 py-1 rounded-full">
                        Debate Challenge ${state.activeDebateIdx + 1} of ${p.debatePrompts.length}
                    </span>
                </div>

                <!-- Debate Card -->
                <div class="bg-white rounded-3xl border border-slate-200 shadow-xl overflow-hidden max-w-3xl mx-auto">
                    <div class="bg-slate-900 p-6 text-white flex items-center space-x-4 border-b border-slate-800">
                        <img src="${p.avatarUrl}" alt="${p.name}" class="w-12 h-12 rounded-xl object-cover ring-2 ring-sky-400 shadow" onerror="this.onerror=null; this.src='https://placehold.co/150x150/0284c7/ffffff?text=${encodeURIComponent(p.name)}';">
                        <div>
                            <div class="flex items-center space-x-2">
                                <h3 class="font-bold text-lg">${p.name}</h3>
                                <span class="text-xs bg-sky-950 text-sky-300 border border-sky-700/50 px-2.5 py-0.5 rounded-full font-medium">
                                    ${p.communityPosition}
                                </span>
                            </div>
                            <p class="text-xs text-sky-400">Social Studies Critical Thinking Evaluation</p>
                        </div>
                    </div>

                    <div class="p-6 sm:p-8 space-y-6">
                        <div class="bg-indigo-50/70 p-5 rounded-2xl border border-indigo-100 text-sm text-indigo-950 font-medium leading-relaxed">
                            <i data-lucide="quote" class="w-5 h-5 text-indigo-400 mb-2"></i>
                            ${prompt.question}
                        </div>

                        <h4 class="text-xs font-bold uppercase tracking-wider text-slate-400">Select the most balanced, well-reasoned G3 Social Studies response:</h4>

                        <div class="space-y-3" id="debate-options-container">
                            ${prompt.options.map((opt, idx) => `
                                <button onclick="selectDebateOption(${idx})" class="w-full text-left p-4 rounded-xl border border-slate-200 hover:border-indigo-500 hover:bg-indigo-50/50 transition-all flex items-start space-x-3 group">
                                    <div class="w-6 h-6 rounded-full bg-slate-100 group-hover:bg-indigo-600 group-hover:text-white text-slate-600 font-bold text-xs flex items-center justify-center shrink-0 mt-0.5 transition">
                                        ${idx + 1}
                                    </div>
                                    <span class="text-xs sm:text-sm font-medium text-slate-700 group-hover:text-slate-900">${opt.text}</span>
                                </button>
                            `).join('')}
                        </div>

                        <!-- Feedback Area -->
                        <div id="debate-feedback" class="hidden p-5 rounded-2xl border space-y-3"></div>
                    </div>
                </div>
            `;
            container.innerHTML = html;
        }

        function selectDebateOption(optIdx) {
            const p = PERSONAS[state.selectedPersonaId];
            const prompt = p.debatePrompts[state.activeDebateIdx];
            const selectedOpt = prompt.options[optIdx];

            const container = document.getElementById('debate-options-container');
            const btns = container.querySelectorAll('button');
            btns.forEach((btn, idx) => {
                btn.disabled = true;
                if (idx === optIdx) {
                    btn.classList.add(selectedOpt.correct ? 'border-emerald-500' : 'border-rose-500', 'bg-slate-50');
                } else {
                    btn.classList.add('opacity-50');
                }
            });

            if (selectedOpt.correct) {
                state.personaProgress[p.id].score += 15;
                saveProgressToStorage();
            }

            const feedbackBox = document.getElementById('debate-feedback');
            feedbackBox.classList.remove('hidden');

            if (selectedOpt.correct) {
                feedbackBox.className = "p-5 rounded-2xl border border-emerald-300 bg-emerald-50/80 text-emerald-900 space-y-3";
                feedbackBox.innerHTML = `
                    <div class="flex items-center space-x-2 font-bold text-sm text-emerald-800">
                        <i data-lucide="check-circle-2" class="w-5 h-5 text-emerald-600"></i>
                        <span>High-Quality Social Studies Response (+15 pts)</span>
                    </div>
                    <p class="text-xs leading-relaxed">${selectedOpt.explanation}</p>
                    <div class="pt-2 flex justify-end">
                        <button onclick="nextDebate()" class="bg-emerald-700 hover:bg-emerald-800 text-white font-semibold text-xs px-5 py-2.5 rounded-xl transition flex items-center space-x-1.5">
                            <span>Next Question</span>
                            <i data-lucide="arrow-right" class="w-4 h-4"></i>
                        </button>
                    </div>
                `;
            } else {
                feedbackBox.className = "p-5 rounded-2xl border border-amber-300 bg-amber-50/80 text-amber-900 space-y-3";
                feedbackBox.innerHTML = `
                    <div class="flex items-center space-x-2 font-bold text-sm text-amber-800">
                        <i data-lucide="help-circle" class="w-5 h-5 text-amber-600"></i>
                        <span>Consider Perspective & Trade-offs</span>
                    </div>
                    <p class="text-xs leading-relaxed">${selectedOpt.explanation}</p>
                    <div class="pt-2 flex justify-end">
                        <button onclick="nextDebate()" class="bg-amber-700 hover:bg-amber-800 text-white font-semibold text-xs px-5 py-2.5 rounded-xl transition flex items-center space-x-1.5">
                            <span>Next Question</span>
                            <i data-lucide="arrow-right" class="w-4 h-4"></i>
                        </button>
                    </div>
                `;
            }
            if (window.lucide) lucide.createIcons();
        }

        function nextDebate() {
            state.activeDebateIdx++;
            renderDebateView(document.getElementById('main-container'));
        }

        function renderGlossaryView(container) {
            const html = `
                <div class="mb-6 flex items-center justify-between">
                    <div>
                        <h2 class="text-2xl font-bold text-slate-900 flex items-center space-x-2">
                            <i data-lucide="book-open" class="w-6 h-6 text-sky-600"></i>
                            <span>Social Studies Concept Manual</span>
                        </h2>
                        <p class="text-xs text-slate-500">Essential terminology for Sec 3 G3 Issue 3: Globalisation</p>
                    </div>
                    <button onclick="navigateTo('hub')" class="bg-slate-100 hover:bg-slate-200 text-slate-700 text-xs font-semibold px-4 py-2 rounded-xl transition">
                        Back to Hub
                    </button>
                </div>

                <div class="grid grid-cols-1 md:grid-cols-2 gap-6 mb-12">
                    ${GLOSSARY_TERMS.map(g => `
                        <div class="bg-white rounded-2xl border border-slate-200 p-6 shadow-sm hover:border-sky-300 transition space-y-3">
                            <h3 class="text-lg font-bold text-slate-900 text-sky-700">${g.term}</h3>
                            <p class="text-xs text-slate-700 leading-relaxed">${g.definition}</p>
                            <div class="bg-slate-50 p-3 rounded-xl border border-slate-100 text-xs text-slate-600 italic">
                                <strong>Example in Context:</strong> "${g.example}"
                            </div>
                        </div>
                    `).join('')}
                </div>
            `;
            container.innerHTML = html;
        }

        let synthesisChart = null;

        function renderSynthesisView(container) {
            const html = `
                <div class="mb-6">
                    <button onclick="navigateTo('hub')" class="inline-flex items-center space-x-1 text-xs font-semibold text-slate-500 hover:text-slate-800">
                        <i data-lucide="chevron-left" class="w-4 h-4"></i>
                        <span>Back to Stakeholder Hub</span>
                    </button>
                </div>

                <div class="bg-gradient-to-r from-slate-900 via-indigo-950 to-slate-900 rounded-3xl p-6 sm:p-8 text-white mb-8 border border-slate-800 shadow-xl">
                    <div class="max-w-3xl">
                        <span class="bg-indigo-500/20 text-indigo-300 border border-indigo-400/30 text-xs font-bold px-3 py-1 rounded-full uppercase tracking-wider inline-block mb-3">
                            Townhall Finale Mode
                        </span>
                        <h2 class="text-2xl sm:text-3xl font-extrabold mb-2">National Policy Balancing Simulation</h2>
                        <p class="text-xs sm:text-sm text-slate-300 leading-relaxed">
                            As a National Policy Advisor, adjust government policy levers below. Watch how trade-offs impact the satisfaction radar across all 6 stakeholders in real time!
                        </p>
                    </div>
                </div>

                <div class="grid grid-cols-1 lg:grid-cols-12 gap-8 mb-12">
                    <!-- Policy Levers Controls -->
                    <div class="lg:col-span-5 bg-white rounded-3xl border border-slate-200 p-6 shadow-lg space-y-6">
                        <h3 class="text-lg font-bold text-slate-900 border-b pb-3 flex items-center space-x-2">
                            <i data-lucide="sliders" class="w-5 h-5 text-indigo-600"></i>
                            <span>Adjust Policy Priorities</span>
                        </h3>

                        <!-- Slider 1 -->
                        <div class="space-y-2">
                            <div class="flex justify-between items-center text-xs font-bold">
                                <label class="text-slate-700">Foreign Direct Investment & MNC Openness</label>
                                <span id="fdi-val" class="text-indigo-600">${state.synthesisPolicies.fdiOpenness}%</span>
                            </div>
                            <input type="range" min="10" max="100" value="${state.synthesisPolicies.fdiOpenness}" id="fdi-slider" oninput="updateSynthesis()" class="w-full h-2 bg-slate-200 rounded-lg appearance-none cursor-pointer accent-indigo-600">
                            <p class="text-[11px] text-slate-500">Higher = Attracts MNCs & Tech Jobs; Lower = Protects local SMEs from direct foreign competition.</p>
                        </div>

                        <!-- Slider 2 -->
                        <div class="space-y-2">
                            <div class="flex justify-between items-center text-xs font-bold">
                                <label class="text-slate-700">Digitalization & Smart Nation Drive Speed</label>
                                <span id="digital-val" class="text-indigo-600">${state.synthesisPolicies.digitalSpeed}%</span>
                            </div>
                            <input type="range" min="10" max="100" value="${state.synthesisPolicies.digitalSpeed}" id="digital-slider" oninput="updateSynthesis()" class="w-full h-2 bg-slate-200 rounded-lg appearance-none cursor-pointer accent-indigo-600">
                            <p class="text-[11px] text-slate-500">Higher = Productivity & Global tech access; Lower = Reduces digital divide stress for non-tech users.</p>
                        </div>

                        <!-- Slider 3 -->
                        <div class="space-y-2">
                            <div class="flex justify-between items-center text-xs font-bold">
                                <label class="text-slate-700">Local Heritage & Cultural Preservation Funding</label>
                                <span id="heritage-val" class="text-indigo-600">${state.synthesisPolicies.heritageProtection}%</span>
                            </div>
                            <input type="range" min="10" max="100" value="${state.synthesisPolicies.heritageProtection}" id="heritage-slider" oninput="updateSynthesis()" class="w-full h-2 bg-slate-200 rounded-lg appearance-none cursor-pointer accent-indigo-600">
                            <p class="text-[11px] text-slate-500">Higher = Safeguards local heritage, hawker culture & local businesses; Lower = Frees up budget for infrastructure.</p>
                        </div>

                        <div class="bg-indigo-50 p-4 rounded-2xl border border-indigo-100 text-xs text-indigo-900 leading-relaxed">
                            <strong>Key Lesson:</strong> No policy decision can satisfy all 6 stakeholders 100%. Social Studies teaches us that governance is about managing trade-offs responsibly.
                        </div>
                    </div>

                    <!-- Stakeholder Radar Chart Output -->
                    <div class="lg:col-span-7 bg-white rounded-3xl border border-slate-200 p-6 shadow-lg flex flex-col justify-between">
                        <div>
                            <h3 class="text-lg font-bold text-slate-900 mb-2 flex items-center space-x-2">
                                <i data-lucide="pie-chart" class="w-5 h-5 text-indigo-600"></i>
                                <span>Stakeholder Impact Radar</span>
                            </h3>
                            <p class="text-xs text-slate-500 mb-4">Estimated Satisfaction Index (%) based on policy trade-offs.</p>
                        </div>

                        <div class="w-full max-w-md mx-auto aspect-square flex items-center justify-center">
                            <canvas id="synthesis-radar-chart"></canvas>
                        </div>

                        <div id="radar-summary-text" class="mt-4 text-xs text-center text-slate-600 font-medium">
                            Adjust the sliders on the left to see live impact balance.
                        </div>
                    </div>
                </div>
            `;
            container.innerHTML = html;

            setTimeout(() => {
                initRadarChart();
                updateSynthesis();
            }, 50);
        }

        function initRadarChart() {
            const ctx = document.getElementById('synthesis-radar-chart')?.getContext('2d');
            if (!ctx) return;

            if (synthesisChart) synthesisChart.destroy();

            synthesisChart = new Chart(ctx, {
                type: 'radar',
                data: {
                    labels: [
                        'Mr. Tan (Heritage SME)', 
                        'Ms. Elena (MNC Investor)', 
                        'Mdm. Rahimah (Parent)', 
                        'Ethan (Youth Talent)', 
                        'Uncle Krishnan (Senior)', 
                        'Ah Hock (Gig Worker)'
                    ],
                    datasets: [{
                        label: 'Stakeholder Satisfaction (%)',
                        data: [50, 50, 50, 50, 50, 50],
                        backgroundColor: 'rgba(99, 102, 241, 0.25)',
                        borderColor: 'rgba(79, 70, 229, 1)',
                        borderWidth: 2,
                        pointBackgroundColor: 'rgba(79, 70, 229, 1)',
                        pointBorderColor: '#fff',
                        pointHoverBackgroundColor: '#fff',
                        pointHoverBorderColor: 'rgba(79, 70, 229, 1)'
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: true,
                    scales: {
                        r: {
                            angleLines: { color: '#e2e8f0' },
                            grid: { color: '#e2e8f0' },
                            pointLabels: { font: { size: 10, weight: 'bold', family: 'Inter' }, color: '#334155' },
                            suggestedMin: 0,
                            suggestedMax: 100
                        }
                    },
                    plugins: {
                        legend: { display: false }
                    }
                }
            });
        }
    </script>
</body>
</html>
