import React, { useState, useMemo } from 'react';
import { 
  ShieldAlert, 
  CheckCircle2, 
  AlertTriangle, 
  Info, 
  Search, 
  Filter, 
  Plus, 
  ChevronRight, 
  User, 
  FileText, 
  Activity, 
  Sparkles, 
  Pill, 
  Layers, 
  Check, 
  RefreshCw, 
  ExternalLink,
  LayoutDashboard,
  ClipboardList,
  Users,
  Settings,
  Bell,
  HelpCircle,
  Menu,
  X,
  Eye,
  Sliders,
  Trash2,
  Edit3,
  Download,
  Printer,
  Lock,
  LogOut,
  ChevronDown,
  AlertCircle,
  FileSpreadsheet,
  Stethoscope,
  Heart,
  Calendar,
  Building,
  CheckSquare,
  Square,
  MessageSquare,
  Send,
  Zap,
  ArrowRight
} from 'lucide-react';

// ==========================================
// 1. EXACT MATCH APP ICON SVG (Matching User Reference)
// ==========================================
const MedsyLogoIcon = ({ size = 36, className = "" }) => (
  <svg 
    width={size} 
    height={size} 
    viewBox="0 0 120 120" 
    fill="none" 
    xmlns="http://www.w3.org/2000/svg"
    className={className}
  >
    <defs>
      <linearGradient id="medsyTealGrad" x1="0%" y1="0%" x2="100%" y2="100%">
        <stop offset="0%" stopColor="#2C6975" />
        <stop offset="100%" stopColor="#174B55" />
      </linearGradient>
      <linearGradient id="medsySwooshGrad" x1="0%" y1="0%" x2="100%" y2="100%">
        <stop offset="0%" stopColor="#68B2A0" />
        <stop offset="50%" stopColor="#CDE0C9" />
        <stop offset="100%" stopColor="#2C6975" />
      </linearGradient>
    </defs>
    
    {/* Stylized Rounded Medical Cross */}
    <rect x="44" y="16" width="32" height="88" rx="14" fill="url(#medsyTealGrad)" />
    <rect x="16" y="44" width="88" height="32" rx="14" fill="url(#medsyTealGrad)" />
    
    {/* Elegant Curved Dual-tone Pill Swoosh Wave */}
    <path 
      d="M 22 76 C 32 94, 58 92, 75 70 C 88 52, 100 42, 104 46 C 108 50, 96 68, 80 85 C 64 102, 35 98, 22 76 Z" 
      fill="url(#medsySwooshGrad)" 
      opacity="0.95"
    />
    
    {/* Embedded Capsule Pill Element */}
    <g transform="translate(62, 38) rotate(35)">
      <rect x="0" y="0" width="32" height="16" rx="8" fill="#FFFFFF" />
      <rect x="16" y="0" width="16" height="16" rx="8" fill="#68B2A0" />
      <line x1="16" y1="0" x2="16" y2="16" stroke="#2C6975" strokeWidth="1.5" />
    </g>

    {/* Sparkle AI Star Element Top Right */}
    <path 
      d="M 94 20 L 96 27 L 103 29 L 96 31 L 94 38 L 92 31 L 85 29 L 92 27 Z" 
      fill="#68B2A0" 
    />
  </svg>
);

// ==========================================
// SYNTHETIC INITIAL DEMO DATA (Adam Miller)
// ==========================================
const INITIAL_PATIENT = {
  id: "PT-10942",
  fullName: "Adam Miller",
  age: 23,
  gender: "Male",
  dob: "2003-05-14",
  mrn: "CF-883920",
  admissionDate: "2026-11-10",
  department: "Pulmonology & Adult CF Care",
  admissionStatus: "Admitted - Inpatient",
  primaryDiagnosis: "Cystic Fibrosis with Acute Pulmonary Exacerbation",
  attendingPhysician: "Dr. Rachel Vance, MD",
  room: "Bed 412-A",
  associatedConditions: [
    "Pancreatic Insufficiency",
    "Cystic Fibrosis-Related Diabetes (CFRD)",
    "Recurrent Pseudomonas Respiratory Infections",
    "Intestinal Malabsorption",
    "Chronic Sinusitis",
    "Fat-Soluble Vitamin Deficiencies (A, D, E, K)"
  ],
  labResults: [
    { test: "FEV1 % Predicted", result: "48%", status: "Abnormal", date: "2026-11-10" },
    { test: "Blood Glucose (Fasting)", result: "164 mg/dL", status: "High", date: "2026-11-11" },
    { test: "HbA1c", result: "7.6%", status: "Elevated", date: "2026-11-10" },
    { test: "Serum Creatinine", result: "0.9 mg/dL", status: "Normal", date: "2026-11-10" },
    { test: "eGFR", result: ">90 mL/min", status: "Normal", date: "2026-11-10" },
    { test: "25-OH Vitamin D", result: "19 ng/mL", status: "Low Deficient", date: "2026-11-10" },
    { test: "Sputum Culture", result: "Pseudomonas aeruginosa (Heavy)", status: "Critical", date: "2026-11-10" }
  ]
};

const INITIAL_HOME_MEDS = [
  { id: "HM-1", genericName: "Dornase Alfa", brandName: "Pulmozyme", dose: "2.5 mg", route: "Inhalation (Nebulizer)", frequency: "Once Daily", status: "Active", source: "Outpatient CF Clinic", indication: "Mucolytic / CF Airway Clearance" },
  { id: "HM-2", genericName: "Hypertonic Saline 7%", brandName: "Nebusal", dose: "4 mL", route: "Inhalation (Nebulizer)", frequency: "Twice Daily", status: "Active", source: "Patient Report", indication: "Airway Hydration" },
  { id: "HM-3", genericName: "Salbutamol (Albuterol)", brandName: "Ventolin", dose: "2 puffs (200 mcg)", route: "Inhalation MDI", frequency: "Q4H PRN", status: "Active", source: "Retail Pharmacy Sync", indication: "Bronchospasm" },
  { id: "HM-4", genericName: "Pancrelipase", brandName: "Creon", dose: "24,000 units", route: "Oral", frequency: "With all meals & snacks", status: "Active", source: "PBM Claims", indication: "Pancreatic Enzyme Replacement" },
  { id: "HM-5", genericName: "Omeprazole", brandName: "Prilosec", dose: "20 mg", route: "Oral", frequency: "Once Daily", status: "Active", source: "Outpatient EMR", indication: "GERD / Enhance Enzyme Efficacy" },
  { id: "HM-6", genericName: "Insulin Lispro", brandName: "Humalog", dose: "Sliding Scale (approx 4-6 units)", route: "Subcutaneous", frequency: "Before Meals", status: "Active", source: "Endocrinology Note", indication: "CFRD Glycemic Control" },
  { id: "HM-7", genericName: "CF Fat-Soluble Vitamins (A, D, E, K)", brandName: "DEKAs Plus", dose: "1 Softgel", route: "Oral", frequency: "Once Daily", status: "Active", source: "Patient Report", indication: "Nutritional Supplement" },
  { id: "HM-8", genericName: "Azithromycin", brandName: "Zithromax", dose: "500 mg", route: "Oral", frequency: "Mon / Wed / Fri", status: "Active", source: "Outpatient EMR", indication: "Anti-inflammatory / Immunomodulation in CF" },
];

const INITIAL_HOSPITAL_MEDS = [
  { id: "HSP-1", genericName: "Ceftazidime", brandName: "Fortaz", dose: "2 g", route: "IV Piggyback", frequency: "Q8H", status: "Ordered", indication: "Pseudomonas Aeruginosa Exacerbation" },
  { id: "HSP-2", genericName: "Tobramycin", brandName: "Nebcin", dose: "10 mg/kg (700 mg)", route: "IV Infusion", frequency: "Once Daily (Once-Daily Dosing)", status: "Ordered", indication: "Pseudomonas Synergy" },
  { id: "HSP-3", genericName: "Meropenem", brandName: "Merrem", dose: "1 g", route: "IV", frequency: "Q8H", status: "Pending Approval", indication: "Broad-spectrum coverage" },
  { id: "HSP-4", genericName: "Co-trimoxazole (TMP-SMX)", brandName: "Bactrim DS", dose: "1 Tablet", route: "Oral", frequency: "Twice Daily", status: "Ordered", indication: "Prophylaxis / Secondary Infection" },
  { id: "HSP-5", genericName: "Dornase Alfa", brandName: "Pulmozyme", dose: "2.5 mg", route: "Inhalation", frequency: "Once Daily", status: "Active Order", indication: "CF Mucolytic" },
  { id: "HSP-6", genericName: "Omeprazole", brandName: "Prilosec", dose: "40 mg", route: "Oral", frequency: "Twice Daily", status: "Active Order", indication: "Acid Suppression" }
];

const INITIAL_ALLERGIES = [
  { id: "ALG-1", substance: "Sulfonamides (Sulfa drugs)", category: "Drug", reaction: "Severe Erythematous Skin Rash & Urticaria", severity: "High / Severe", recordedDate: "2023-04-12", status: "Confirmed" }
];

const INITIAL_DISCREPANCIES = [
  {
    id: "DISC-101",
    medication: "Pancrelipase (Creon)",
    homeDetails: "24,000 Units Oral w/ Meals & Snacks",
    hospitalDetails: "NOT ORDERED IN ADMISSION LIST",
    type: "Omission of Essential Therapy",
    severity: "Critical",
    clinicalExplanation: "Patient has severe pancreatic insufficiency. Omission of pancreatic enzymes will result in acute fat malabsorption, severe abdominal pain, steatorrhea, and inability to absorb oral fat-soluble medications.",
    recommendation: "Re-order Creon 24,000 units oral with all meals and snacks immediately.",
    status: "Pending Action",
    category: "Omission"
  },
  {
    id: "DISC-102",
    medication: "Co-trimoxazole (TMP-SMX / Bactrim DS)",
    homeDetails: "Not on home list",
    hospitalDetails: "1 Tab Oral Twice Daily (Ordered)",
    type: "Allergy Conflict (Sulfa)",
    severity: "Critical",
    clinicalExplanation: "Patient has a documented HIGH SEVERITY allergic reaction to Sulfonamides (Severe Skin Rash & Urticaria). Ordering TMP-SMX creates a direct life-threatening allergy conflict.",
    recommendation: "DISCONTINUE TMP-SMX immediately. Select non-sulfa coverage if indicated.",
    status: "Pending Action",
    category: "Allergy Conflict"
  },
  {
    id: "DISC-103",
    medication: "Omeprazole",
    homeDetails: "20 mg Oral Once Daily",
    hospitalDetails: "40 mg Oral Twice Daily",
    type: "Dose & Frequency Discrepancy",
    severity: "Moderate",
    clinicalExplanation: "Hospital order is 4x the home maintenance regimen. Verify if dose escalation is intentional for acute GI symptoms or an accidental intake error.",
    recommendation: "Clarify intent with prescribing team. Standard home dose is 20 mg QD.",
    status: "Pending Action",
    category: "Dose Mismatch"
  },
  {
    id: "DISC-104",
    medication: "Meropenem + Ceftazidime",
    homeDetails: "N/A",
    hospitalDetails: "Ceftazidime 2g Q8H IV + Meropenem 1g Q8H IV",
    type: "Duplicate Beta-Lactam Coverage",
    severity: "High",
    clinicalExplanation: "Concurrent double beta-lactam therapy for Pseudomonas is generally redundant and increases toxicity/resistance without added efficacy compared to Ceftazidime + Tobramycin.",
    recommendation: "Discontinue Meropenem; proceed with Ceftazidime + IV Tobramycin synergy.",
    status: "Pending Action",
    category: "Duplicate Therapy"
  },
  {
    id: "DISC-105",
    medication: "DEKAs Plus (Fat-Soluble Vitamins)",
    homeDetails: "1 Softgel Oral Daily",
    hospitalDetails: "NOT ORDERED",
    type: "Omission of Maintenance Supplement",
    severity: "Informational",
    clinicalExplanation: "Home vitamin supplementation omitted. Serum 25-OH Vitamin D is already low (19 ng/mL).",
    recommendation: "Resume DEKAs Plus 1 softgel daily during hospital stay.",
    status: "Pending Action",
    category: "Omission"
  }
];

export default function MedsyApp() {
  const [isAuthenticated, setIsAuthenticated] = useState(true);
  const [loginEmail, setLoginEmail] = useState("s.jenkins@stjude-hospital.org");
  const [loginPassword, setLoginPassword] = useState("••••••••••••");

  const [activeTab, setActiveTab] = useState("dashboard");
  const [searchQuery, setSearchQuery] = useState("");

  const [patient, setPatient] = useState(INITIAL_PATIENT);
  const [homeMeds, setHomeMeds] = useState(INITIAL_HOME_MEDS);
  const [hospitalMeds, setHospitalMeds] = useState(INITIAL_HOSPITAL_MEDS);
  const [allergies, setAllergies] = useState(INITIAL_ALLERGIES);
  const [discrepancies, setDiscrepancies] = useState(INITIAL_DISCREPANCIES);

  const [pharmacistNotes, setPharmacistNotes] = useState("");
  const [reviewDecision, setReviewDecision] = useState("Requires Physician Clarification");
  const [isSignedOff, setIsSignedOff] = useState(false);
  const [pharmacistName, setPharmacistName] = useState("Dr. Sarah Jenkins, PharmD, BCPS");

  const [showAddMedModal, setShowAddMedModal] = useState(false);
  const [medModalType, setMedModalType] = useState("home");
  const [newData, setNewData] = useState({ genericName: "", brandName: "", dose: "", route: "Oral", frequency: "Once Daily", indication: "" });
  
  const [showAiModal, setShowAiModal] = useState(false);
  const [aiAnalysisRunning, setAiAnalysisRunning] = useState(false);
  const [aiOutput, setAiOutput] = useState(null);

  const [discrepancyFilter, setDiscrepancyFilter] = useState("ALL");

  const filteredDiscrepancies = useMemo(() => {
    if (discrepancyFilter === "ALL") return discrepancies;
    if (discrepancyFilter === "ACTION") return discrepancies.filter(d => d.status !== "Resolved");
    return discrepancies.filter(d => d.severity === discrepancyFilter || d.category === discrepancyFilter);
  }, [discrepancies, discrepancyFilter]);

  const handleResolveDiscrepancy = (id, resolutionText = "Resolved & Clarified by Pharmacist") => {
    setDiscrepancies(prev => prev.map(d => d.id === id ? { ...d, status: "Resolved", resolution: resolutionText } : d));
  };

  const handleSaveNewMedication = (e) => {
    e.preventDefault();
    const newId = `${medModalType.toUpperCase()}-${Date.now()}`;
    const newEntry = {
      id: newId,
      genericName: newData.genericName || "Unnamed Drug",
      brandName: newData.brandName || "Generic",
      dose: newData.dose || "10 mg",
      route: newData.route || "Oral",
      frequency: newData.frequency || "Once Daily",
      status: "Active",
      source: medModalType === "home" ? "Pharmacist Added" : "Inpatient Order Entry",
      indication: newData.indication || "Clinical Indication"
    };

    if (medModalType === "home") {
      setHomeMeds([...homeMeds, newEntry]);
    } else {
      setHospitalMeds([...hospitalMeds, newEntry]);
    }

    setShowAddMedModal(false);
    setNewData({ genericName: "", brandName: "", dose: "", route: "Oral", frequency: "Once Daily", indication: "" });
  };

  const handleRunAiAnalysis = () => {
    setAiAnalysisRunning(true);
    setShowAiModal(true);
    setTimeout(() => {
      setAiAnalysisRunning(false);
      setAiOutput({
        summary: "High-risk clinical pattern identified for Adam Miller (Cystic Fibrosis Exacerbation). Direct safety conflicts detected between inpatient order entry and outpatient profile.",
        findings: [
          "CRITICAL ALLERGY ALERT: Active order for TMP-SMX (Bactrim) directly violates documented severe Sulfa drug allergy.",
          "CRITICAL OMISSION: Pancrelipase (Creon) is missing from admission orders. Patient is at high risk of rapid gastrointestinal malabsorption and enzyme deficiency symptoms.",
          "ANTIMICROBIAL STEWARDSHIP: Redundant double beta-lactam therapy (Meropenem + Ceftazidime) noted. Recommend discontinuing Meropenem and maintaining IV Tobramycin for synergy."
        ],
        verificationSteps: [
          "Step 1: Discontinue TMP-SMX order in hospital EHR immediately.",
          "Step 2: Enter inpatient order for Creon 24,000 units with all meals & snacks.",
          "Step 3: Page Dr. Vance regarding Meropenem discontinuation."
        ]
      });
    }, 1200);
  };

  if (!isAuthenticated) {
    return (
      <div className="min-h-screen bg-[#123940] flex items-center justify-center p-4 font-sans text-white">
        <div className="max-w-md w-full bg-[#174B55] border border-[#2C6975] rounded-2xl shadow-2xl overflow-hidden">
          <div className="p-8 text-center bg-[#2C6975]/40 border-b border-[#2C6975]">
            <div className="w-16 h-16 bg-[#2C6975] rounded-2xl mx-auto flex items-center justify-center shadow-lg border border-[#68B2A0]/40 mb-4">
              <MedsyLogoIcon size={44} />
            </div>
            <h1 className="text-3xl font-extrabold tracking-tight text-white">MEDSY</h1>
            <p className="text-xs text-[#68B2A0] font-semibold mt-1 uppercase tracking-wider">AI-Powered Medication Reconciliation Assistant</p>
          </div>
          <form onSubmit={(e) => { e.preventDefault(); setIsAuthenticated(true); }} className="p-6 space-y-4">
            <div>
              <label className="block text-xs font-semibold text-[#CDE0C9] mb-1">Clinical User Email</label>
              <input 
                type="email" 
                value={loginEmail}
                onChange={(e) => setLoginEmail(e.target.value)}
                className="w-full bg-[#123940] border border-[#2C6975] rounded-lg px-3 py-2 text-sm text-white focus:outline-none focus:border-[#68B2A0]"
                required
              />
            </div>
            <div>
              <label className="block text-xs font-semibold text-[#CDE0C9] mb-1">Password</label>
              <input 
                type="password" 
                value={loginPassword}
                onChange={(e) => setLoginPassword(e.target.value)}
                className="w-full bg-[#123940] border border-[#2C6975] rounded-lg px-3 py-2 text-sm text-white focus:outline-none focus:border-[#68B2A0]"
                required
              />
            </div>
            <button 
              type="submit"
              className="w-full bg-[#2C6975] hover:bg-[#68B2A0] text-white hover:text-[#123940] transition-colors py-2.5 rounded-xl font-bold text-sm shadow-md flex items-center justify-center space-x-2"
            >
              <Lock className="w-4 h-4" />
              <span>Sign In to Clinical Portal</span>
            </button>
          </form>
        </div>
      </div>
    );
  }

  return (
    <div className="min-h-screen bg-[#F4F8F6] text-[#123940] font-sans antialiased flex flex-col">
      
      {/* GLOBAL TOP NAVIGATION BAR */}
      <header className="bg-[#174B55] text-white border-b border-[#2C6975] sticky top-0 z-30 shadow-md">
        <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 h-16 flex items-center justify-between">
          <div className="flex items-center space-x-3 cursor-pointer" onClick={() => setActiveTab('dashboard')}>
            <div className="w-10 h-10 rounded-xl bg-[#2C6975] flex items-center justify-center text-white shadow-inner border border-[#68B2A0]/30">
              <MedsyLogoIcon size={28} />
            </div>
            <div>
              <div className="flex items-center space-x-2">
                <span className="font-extrabold text-xl tracking-tight text-white">MEDSY</span>
                <span className="bg-[#68B2A0]/20 text-[#68B2A0] text-[10px] font-semibold px-2 py-0.5 rounded-full border border-[#68B2A0]/30 uppercase tracking-wider">Clinical AI</span>
              </div>
              <p className="text-[11px] text-[#CDE0C9] hidden sm:block">AI-Powered Medication Reconciliation Assistant</p>
            </div>
          </div>

          <div className="hidden md:flex flex-1 max-w-md mx-8 relative">
            <Search className="w-4 h-4 absolute left-3 top-3 text-[#68B2A0]" />
            <input 
              type="text" 
              placeholder="Search patient name, MRN, drug name..."
              value={searchQuery}
              onChange={(e) => setSearchQuery(e.target.value)}
              className="w-full bg-[#123940] border border-[#2C6975] rounded-xl pl-9 pr-4 py-1.5 text-xs text-white placeholder-[#CDE0C9]/60 focus:outline-none focus:border-[#68B2A0]"
            />
          </div>

          <div className="flex items-center space-x-3">
            <button 
              onClick={handleRunAiAnalysis}
              className="bg-[#2C6975] hover:bg-[#68B2A0] hover:text-[#123940] text-white px-3 py-1.5 rounded-xl text-xs font-semibold flex items-center space-x-1.5 transition-all shadow-sm border border-[#68B2A0]/40"
            >
              <Sparkles className="w-3.5 h-3.5 text-[#68B2A0]" />
              <span className="hidden sm:inline">AI Analysis</span>
            </button>
            <div className="flex items-center space-x-3 border-l border-[#2C6975] pl-3">
              <div className="text-right hidden lg:block text-xs">
                <p className="font-semibold text-white">{pharmacistName}</p>
                <p className="text-[#CDE0C9] text-[10px]">Lead Clinical Pharmacist</p>
              </div>
              <button 
                onClick={() => setIsAuthenticated(false)}
                className="w-8 h-8 rounded-full bg-[#2C6975] border border-[#68B2A0] flex items-center justify-center font-bold text-white text-xs hover:bg-red-700 transition-colors"
                title="Logout"
              >
                SJ
              </button>
            </div>
          </div>
        </div>
      </header>

      {/* APP BODY */}
      <div className="flex-1 flex overflow-hidden max-w-7xl mx-auto w-full">
        
        {/* SIDEBAR */}
        <aside className="w-64 bg-[#174B55] text-white flex-col justify-between hidden md:flex border-r border-[#2C6975] shrink-0">
          <div className="p-4 space-y-6">
            <div 
              onClick={() => setActiveTab('patient_profile')}
              className="bg-[#123940] rounded-xl p-3 border border-[#2C6975] cursor-pointer hover:border-[#68B2A0] transition-colors"
            >
              <div className="flex justify-between items-center mb-1">
                <span className="text-[10px] text-[#68B2A0] font-bold uppercase tracking-wider">Active Patient File</span>
                <span className="bg-red-500/20 text-red-300 text-[10px] font-bold px-1.5 py-0.5 rounded border border-red-500/40">High Risk</span>
              </div>
              <h4 className="font-bold text-sm text-white">{patient.fullName}</h4>
              <p className="text-xs text-[#CDE0C9]">{patient.age}y M • MRN: {patient.mrn}</p>
            </div>

            <nav className="space-y-1">
              {[
                { id: 'dashboard', label: 'Dashboard', icon: LayoutDashboard },
                { id: 'patients', label: 'Patients Queue', icon: Users },
                { id: 'patient_profile', label: 'Patient Profile & History', icon: User },
                { id: 'med_reconcile', label: 'Medication Reconciliation', icon: ClipboardList, badge: `${discrepancies.filter(d=>d.status!=='Resolved').length}` },
                { id: 'ai_analysis', label: 'AI Clinical Analysis', icon: Sparkles },
                { id: 'alerts', label: 'Clinical Alerts', icon: ShieldAlert },
                { id: 'review', label: 'Pharmacist Review', icon: Stethoscope },
                { id: 'reports', label: 'Final Audit Report', icon: FileText },
                { id: 'settings', label: 'System Settings', icon: Settings },
              ].map((item) => {
                const Icon = item.icon;
                const isActive = activeTab === item.id;
                return (
                  <button
                    key={item.id}
                    onClick={() => setActiveTab(item.id)}
                    className={`w-full flex items-center justify-between px-3 py-2.5 rounded-xl text-xs font-medium transition-all ${
                      isActive
                        ? 'bg-[#2C6975] text-white font-bold border-l-4 border-[#68B2A0] shadow-sm'
                        : 'text-[#CDE0C9] hover:bg-[#123940] hover:text-white'
                    }`}
                  >
                    <div className="flex items-center space-x-2.5">
                      <Icon className={`w-4 h-4 ${isActive ? 'text-[#68B2A0]' : 'text-[#CDE0C9]'}`} />
                      <span>{item.label}</span>
                    </div>
                    {item.badge && (
                      <span className="text-[10px] px-2 py-0.5 rounded-full bg-[#68B2A0] text-[#123940] font-bold">
                        {item.badge}
                      </span>
                    )}
                  </button>
                );
              })}
            </nav>
          </div>
        </aside>

        {/* WORKSPACE */}
        <main className="flex-1 overflow-y-auto p-4 sm:p-6 lg:p-8 space-y-6">
          
          <div className="bg-[#E0ECDE] border border-[#68B2A0]/40 text-[#123940] px-4 py-2 rounded-xl text-xs flex items-center justify-between shadow-sm">
            <div className="flex items-center space-x-2">
              <Info className="w-4 h-4 text-[#2C6975] shrink-0" />
              <span><strong className="font-bold">SYNTHETIC CLINICAL DEMONSTRATION MODE:</strong> Viewing patient profile for <strong>Adam Miller (Cystic Fibrosis Exacerbation)</strong>.</span>
            </div>
          </div>

          {activeTab === 'dashboard' && (
            <div className="space-y-6">
              <div className="bg-white p-6 rounded-2xl border border-[#CDE0C9] shadow-sm flex flex-col md:flex-row justify-between md:items-center gap-4">
                <div className="flex items-center space-x-4">
                  <div className="w-14 h-14 rounded-2xl bg-[#174B55] text-[#68B2A0] flex items-center justify-center font-extrabold text-xl shadow-inner border border-[#68B2A0]/30">
                    AM
                  </div>
                  <div>
                    <h1 className="text-2xl font-bold text-[#123940]">{patient.fullName}</h1>
                    <p className="text-xs text-gray-500 mt-1">
                      {patient.age}y Male • {patient.department} • Room: {patient.room}
                    </p>
                  </div>
                </div>

                <div className="flex items-center space-x-2">
                  <button 
                    onClick={() => setActiveTab('med_reconcile')}
                    className="bg-[#2C6975] hover:bg-[#174B55] text-white px-4 py-2.5 rounded-xl text-xs font-bold shadow-md transition-all flex items-center space-x-2"
                  >
                    <ClipboardList className="w-4 h-4 text-[#68B2A0]" />
                    <span>Open Reconciliation Engine</span>
                  </button>
                </div>
              </div>

              <div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4">
                <div className="bg-white p-5 rounded-2xl border border-[#CDE0C9] shadow-sm">
                  <span className="text-xs font-semibold text-gray-500">Home Meds Reported</span>
                  <h3 className="text-3xl font-extrabold text-[#123940] mt-2">{homeMeds.length}</h3>
                </div>
                <div className="bg-white p-5 rounded-2xl border border-[#CDE0C9] shadow-sm">
                  <span className="text-xs font-semibold text-gray-500">Hospital Orders</span>
                  <h3 className="text-3xl font-extrabold text-[#123940] mt-2">{hospitalMeds.length}</h3>
                </div>
                <div className="bg-white p-5 rounded-2xl border border-red-200 shadow-sm">
                  <span className="text-xs font-bold text-red-600">Critical Discrepancies</span>
                  <h3 className="text-3xl font-extrabold text-red-700 mt-2">
                    {discrepancies.filter(d => d.severity === 'Critical' && d.status !== 'Resolved').length}
                  </h3>
                </div>
                <div className="bg-white p-5 rounded-2xl border border-[#CDE0C9] shadow-sm">
                  <span className="text-xs font-semibold text-gray-500">Review Status</span>
                  <h3 className="text-xl font-bold text-[#123940] mt-2">
                    {isSignedOff ? "Completed" : "Action Needed"}
                  </h3>
                </div>
              </div>
            </div>
          )}

          {activeTab === 'med_reconcile' && (
            <div className="space-y-6">
              <div className="bg-white p-5 rounded-2xl border border-[#CDE0C9] shadow-sm flex justify-between items-center">
                <h2 className="text-xl font-extrabold text-[#123940]">Medication Reconciliation Engine</h2>
              </div>
              <div className="space-y-4">
                {discrepancies.map((item) => (
                  <div key={item.id} className="bg-white rounded-2xl border border-[#CDE0C9] shadow-sm p-5 space-y-4">
                    <div className="flex justify-between items-center">
                      <span className="bg-red-100 text-red-800 font-bold px-3 py-1 rounded-full text-xs">CRITICAL SAFETY ALERT</span>
                      <span className="text-xs text-gray-400">ID: {item.id}</span>
                    </div>
                    <div className="grid grid-cols-1 md:grid-cols-3 gap-4">
                      <div>
                        <h3 className="text-lg font-extrabold text-[#123940]">{item.medication}</h3>
                        <p className="text-xs font-bold text-red-600">{item.type}</p>
                      </div>
                      <div className="p-3 bg-[#F4F8F6] rounded-xl border text-xs">
                        <p className="font-bold text-[#2C6975]">Home Regimen:</p>
                        <p>{item.homeDetails}</p>
                      </div>
                      <div className="p-3 bg-[#E0ECDE]/50 rounded-xl border text-xs">
                        <p className="font-bold text-[#174B55]">Hospital Order:</p>
                        <p>{item.hospitalDetails}</p>
                      </div>
                    </div>
                  </div>
                ))}
              </div>
            </div>
          )}

        </main>
      </div>

    </div>
  );
}
