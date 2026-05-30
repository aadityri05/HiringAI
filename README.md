# HiringAI

import { useState, useCallback } from "react";

const SAMPLE_CANDIDATES = [
  {
    id: "C001", name: "Priya Sharma", email: "priya.sharma@email.com", phone: "+91 98765 43210",
    city: "Bangalore", college: "IIT Bombay", role: "Frontend Developer",
    skills: ["React", "TypeScript", "CSS", "Node.js"], experience: "2 years",
    education: "B.Tech CSE", matchScore: 91, status: "Shortlisted",
    summary: "Strong React developer with solid TypeScript skills and relevant project experience.",
    appliedDate: "2025-05-28", resumeLink: "#", notes: ""
  },
  {
    id: "C002", name: "Arjun Mehta", email: "arjun.m@email.com", phone: "+91 87654 32109",
    city: "Delhi", college: "Delhi University", role: "Backend Developer",
    skills: ["Python", "FastAPI", "PostgreSQL"], experience: "1 year",
    education: "B.Sc CS", matchScore: 62, status: "Manual Review",
    summary: "Some relevant backend skills but limited production experience.",
    appliedDate: "2025-05-27", resumeLink: "#", notes: ""
  },
  {
    id: "C003", name: "Sneha Rao", email: "sneha.rao@email.com", phone: "+91 76543 21098",
    city: "Hyderabad", college: "BITS Pilani", role: "ML Engineer",
    skills: ["Python", "TensorFlow", "NLP", "PyTorch", "spaCy"], experience: "3 years",
    education: "M.Tech AI", matchScore: 95, status: "Shortlisted",
    summary: "Exceptional ML background with direct NLP experience. Strong fit for the role.",
    appliedDate: "2025-05-26", resumeLink: "#", notes: ""
  },
];

const STATUS_CONFIG = {
  "Shortlisted":   { bg: "#0d2a1f", text: "#00e676", border: "#00e676" },
  "Rejected":      { bg: "#2a0d0d", text: "#ff5252", border: "#ff5252" },
  "Manual Review": { bg: "#2a2200", text: "#ffd600", border: "#ffd600" },
  "Parsing":       { bg: "#0d1a2a", text: "#40c4ff", border: "#40c4ff" },
};

function ScoreBadge({ score }) {
  const color = score >= 80 ? "#00e676" : score >= 60 ? "#ffd600" : "#ff5252";
  return (
    <div style={{ width: 48, height: 48, borderRadius: "50%", border: `2px solid ${color}`, display: "flex", alignItems: "center", justifyContent: "center", fontFamily: "'Space Mono', monospace", fontWeight: 700, fontSize: 13, color, flexShrink: 0, boxShadow: `0 0 10px ${color}33` }}>
      {score}
    </div>
  );
}

function StatusPill({ status }) {
  const cfg = STATUS_CONFIG[status] || STATUS_CONFIG["Manual Review"];
  return (
    <span style={{ background: cfg.bg, color: cfg.text, border: `1px solid ${cfg.border}`, borderRadius: 4, padding: "2px 10px", fontSize: 11, fontFamily: "'Space Mono', monospace", fontWeight: 700, letterSpacing: 1, whiteSpace: "nowrap" }}>
      {status.toUpperCase()}
    </span>
  );
}

function UploadForm({ onSubmit }) {
  const [form, setForm] = useState({ name: "", email: "", phone: "", city: "", college: "", role: "" });
  const [file, setFile] = useState(null);
  const [dragging, setDragging] = useState(false);
  const roles = ["Frontend Developer", "Backend Developer", "ML Engineer", "Full Stack Developer", "DevOps Engineer", "Data Analyst"];

  const handleDrop = useCallback((e) => {
    e.preventDefault(); setDragging(false);
    const f = e.dataTransfer.files[0];
    if (f && f.type === "application/pdf") setFile(f);
  }, []);

  const handleSubmit = () => {
    if (!form.name || !form.email || !form.role || !file) { alert("Please fill all fields and upload your resume."); return; }
    onSubmit({ ...form, file });
  };

  return (
    <div style={{ maxWidth: 600, margin: "0 auto" }}>
      <div style={{ marginBottom: 32, textAlign: "center" }}>
        <div style={{ fontSize: 11, letterSpacing: 4, color: "#666", fontFamily: "'Space Mono', monospace", marginBottom: 8 }}>STEP 01 / APPLICATION</div>
        <h2 style={{ fontSize: 28, fontWeight: 700, color: "#f0f0f0", margin: 0 }}>Submit Your Application</h2>
        <p style={{ color: "#666", marginTop: 8, fontSize: 14 }}>Our AI will screen and score your resume automatically</p>
      </div>
      <div style={{ display: "grid", gridTemplateColumns: "1fr 1fr", gap: 16, marginBottom: 16 }}>
        {[["Full Name","name","text"],["Email Address","email","email"],["Phone Number","phone","tel"],["City","city","text"],["College / University","college","text"]].map(([label, key, type]) => (
          <div key={key} style={key === "college" ? { gridColumn: "1/-1" } : {}}>
            <label style={{ display: "block", fontSize: 11, color: "#888", marginBottom: 6, fontFamily: "'Space Mono', monospace", letterSpacing: 1 }}>{label.toUpperCase()}</label>
            <input type={type} placeholder={label} value={form[key]} onChange={e => setForm(f => ({ ...f, [key]: e.target.value }))}
              style={{ width: "100%", background: "#111", border: "1px solid #2a2a2a", borderRadius: 6, padding: "10px 14px", color: "#f0f0f0", fontSize: 14, outline: "none", boxSizing: "border-box", fontFamily: "inherit" }}
              onFocus={e => e.target.style.borderColor = "#00e676"} onBlur={e => e.target.style.borderColor = "#2a2a2a"} />
          </div>
        ))}
        <div style={{ gridColumn: "1/-1" }}>
          <label style={{ display: "block", fontSize: 11, color: "#888", marginBottom: 6, fontFamily: "'Space Mono', monospace", letterSpacing: 1 }}>ROLE APPLIED FOR</label>
          <select value={form.role} onChange={e => setForm(f => ({ ...f, role: e.target.value }))}
            style={{ width: "100%", background: "#111", border: "1px solid #2a2a2a", borderRadius: 6, padding: "10px 14px", color: form.role ? "#f0f0f0" : "#666", fontSize: 14, outline: "none", boxSizing: "border-box", fontFamily: "inherit" }}>
            <option value="" disabled>Select a role</option>
            {roles.map(r => <option key={r} value={r}>{r}</option>)}
          </select>
        </div>
      </div>
      <div onDragOver={e => { e.preventDefault(); setDragging(true); }} onDragLeave={() => setDragging(false)} onDrop={handleDrop}
        onClick={() => document.getElementById("resume-input").click()}
        style={{ border: `2px dashed ${dragging ? "#00e676" : file ? "#00e676" : "#333"}`, borderRadius: 10, padding: "32px 24px", textAlign: "center", cursor: "pointer", background: dragging ? "#0d2a1f" : file ? "#0d1f14" : "transparent", marginBottom: 24 }}>
        <input id="resume-input" type="file" accept=".pdf" style={{ display: "none" }} onChange={e => setFile(e.target.files[0])} />
        <div style={{ fontSize: 32, marginBottom: 8 }}>{file ? "✅" : "📄"}</div>
        <div style={{ color: file ? "#00e676" : "#888", fontFamily: "'Space Mono', monospace", fontSize: 13 }}>{file ? file.name : "Drop your resume here or click to browse"}</div>
        <div style={{ color: "#555", fontSize: 12, marginTop: 4 }}>PDF only, max 10MB</div>
      </div>
      <button onClick={handleSubmit} style={{ width: "100%", padding: "14px", background: "#00e676", border: "none", borderRadius: 8, color: "#0a0a0a", fontWeight: 700, fontSize: 15, cursor: "pointer", fontFamily: "'Space Mono', monospace", letterSpacing: 1 }}>
        SUBMIT APPLICATION →
      </button>
    </div>
  );
}

function ParsingView({ candidate, onComplete }) {
  const [stage, setStage] = useState(0);
  const [result, setResult] = useState(null);
  const [loading, setLoading] = useState(false);
  const steps = ["Extracting resume text...","Identifying skills and experience...","Scoring role fit...","Generating candidate summary...","Determining status..."];

  const runParsing = async () => {
    setLoading(true);
    for (let i = 0; i < steps.length; i++) { setStage(i); await new Promise(r => setTimeout(r, 700)); }
    const mockResults = [
      { skills: ["React","Node.js","TypeScript","MongoDB"], experience: "2 years", education: "B.Tech CSE", matchScore: 88, status: "Shortlisted", summary: "Strong frontend skills with full-stack exposure. Good fit for the role." },
      { skills: ["Python","FastAPI","SQL","Docker"], experience: "3 years", education: "M.Tech CS", matchScore: 79, status: "Shortlisted", summary: "Solid backend experience with modern stack. Recommended for interview." },
      { skills: ["Java","Spring Boot","MySQL"], experience: "6 months", education: "B.Sc IT", matchScore: 55, status: "Manual Review", summary: "Limited experience but relevant skills. Manual review recommended." },
      { skills: ["HTML","CSS","jQuery"], experience: "Fresher", education: "BCA", matchScore: 38, status: "Rejected", summary: "Insufficient skills for the applied role at this time." },
    ];
    const r = mockResults[Math.floor(Math.random() * mockResults.length)];
    setResult(r);
    setLoading(false);
  };

  if (!loading && stage === 0 && !result) {
    return (
      <div style={{ maxWidth: 500, margin: "0 auto", textAlign: "center" }}>
        <div style={{ fontSize: 11, letterSpacing: 4, color: "#666", fontFamily: "'Space Mono', monospace", marginBottom: 8 }}>STEP 02 / ANALYSIS</div>
        <h2 style={{ fontSize: 28, fontWeight: 700, color: "#f0f0f0", marginBottom: 8 }}>Ready to Analyze</h2>
        <p style={{ color: "#666", marginBottom: 32 }}>Resume received for <strong style={{ color: "#00e676" }}>{candidate.name}</strong></p>
        <button onClick={runParsing} style={{ padding: "14px 40px", background: "#00e676", border: "none", borderRadius: 8, color: "#0a0a0a", fontWeight: 700, fontSize: 14, cursor: "pointer", fontFamily: "'Space Mono', monospace" }}>
          RUN AI SCREENING →
        </button>
      </div>
    );
  }

  if (loading || !result) {
    return (
      <div style={{ maxWidth: 480, margin: "0 auto", textAlign: "center" }}>
        <div style={{ fontSize: 40, marginBottom: 16 }}>⚙️</div>
        <h3 style={{ color: "#f0f0f0", marginBottom: 24 }}>AI Screening in Progress</h3>
        {steps.map((s, i) => (
          <div key={i} style={{ display: "flex", alignItems: "center", gap: 12, padding: "10px 0", opacity: i > stage ? 0.3 : 1 }}>
            <span style={{ fontSize: 16 }}>{i < stage ? "✅" : i === stage ? "⏳" : "○"}</span>
            <span style={{ color: i < stage ? "#00e676" : i === stage ? "#ffd600" : "#555", fontSize: 13, fontFamily: "'Space Mono', monospace" }}>{s}</span>
          </div>
        ))}
      </div>
    );
  }

  const cfg = STATUS_CONFIG[result.status];
  return (
    <div style={{ maxWidth: 560, margin: "0 auto" }}>
      <div style={{ textAlign: "center", marginBottom: 32 }}>
        <div style={{ fontSize: 11, letterSpacing: 4, color: "#666", fontFamily: "'Space Mono', monospace", marginBottom: 8 }}>STEP 03 / RESULT</div>
        <h2 style={{ fontSize: 28, fontWeight: 700, color: "#f0f0f0", margin: 0 }}>Screening Complete</h2>
      </div>
      <div style={{ background: "#111", border: "1px solid #222", borderRadius: 12, padding: 24, marginBottom: 20 }}>
        <div style={{ display: "flex", justifyContent: "space-between", alignItems: "flex-start", marginBottom: 20 }}>
          <div>
            <div style={{ fontSize: 20, fontWeight: 700, color: "#f0f0f0" }}>{candidate.name}</div>
            <div style={{ color: "#666", fontSize: 13 }}>{candidate.role} · {candidate.college}</div>
          </div>
          <div style={{ textAlign: "center" }}>
            <ScoreBadge score={result.matchScore} />
            <div style={{ fontSize: 10, color: "#555", marginTop: 4, fontFamily: "'Space Mono', monospace" }}>MATCH</div>
          </div>
        </div>
        <div style={{ background: cfg.bg, border: `1px solid ${cfg.border}`, borderRadius: 8, padding: 16, marginBottom: 20 }}>
          <div style={{ color: cfg.text, fontWeight: 700, fontFamily: "'Space Mono', monospace", fontSize: 13, marginBottom: 6 }}>
            {result.status === "Shortlisted" ? "🎉" : result.status === "Rejected" ? "❌" : "⚠️"} {result.status.toUpperCase()}
          </div>
          <div style={{ color: "#ccc", fontSize: 13 }}>{result.summary}</div>
        </div>
        <div style={{ display: "flex", flexWrap: "wrap", gap: 6, marginBottom: 16 }}>
          {result.skills.map(s => (<span key={s} style={{ background: "#1a2a1a", border: "1px solid #2a4a2a", color: "#00e676", padding: "3px 10px", borderRadius: 4, fontSize: 11, fontFamily: "'Space Mono', monospace" }}>{s}</span>))}
        </div>
        {result.status === "Shortlisted" && (
          <div style={{ background: "#0a1a0a", border: "1px solid #1a3a1a", borderRadius: 8, padding: 16 }}>
            <div style={{ fontSize: 11, color: "#555", fontFamily: "'Space Mono', monospace", marginBottom: 8 }}>📧 AUTO-GENERATED EMAIL</div>
            <div style={{ color: "#aaa", fontSize: 13, lineHeight: 1.7 }}>
              <strong style={{ color: "#f0f0f0" }}>Subject:</strong> You've been shortlisted — {candidate.role}<br /><br />
              Dear {candidate.name},<br /><br />
              Congratulations! You've been shortlisted for <strong>{candidate.role}</strong>. We'll send a calendar invite within 24 hours.<br /><br />
              Best regards, Talent Team
            </div>
          </div>
        )}
      </div>
      <button onClick={() => onComplete({ ...candidate, ...result, id: "C" + String(Math.floor(Math.random() * 900) + 100), appliedDate: new Date().toISOString().split("T")[0], resumeLink: "#", notes: "" })}
        style={{ width: "100%", padding: 14, background: "#00e676", border: "none", borderRadius: 8, color: "#0a0a0a", fontWeight: 700, fontSize: 14, cursor: "pointer", fontFamily: "'Space Mono', monospace" }}>
        VIEW IN DASHBOARD →
      </button>
    </div>
  );
}

function Dashboard({ candidates, setCandidates }) {
  const [filter, setFilter] = useState("All");
  const [search, setSearch] = useState("");
  const [selected, setSelected] = useState(null);
  const [editNote, setEditNote] = useState("");
  const statuses = ["All", "Shortlisted", "Manual Review", "Rejected"];
  const filtered = candidates.filter(c => (filter === "All" || c.status === filter) && (!search || c.name.toLowerCase().includes(search.toLowerCase()) || c.role.toLowerCase().includes(search.toLowerCase())));
  const stats = { total: candidates.length, shortlisted: candidates.filter(c => c.status === "Shortlisted").length, review: candidates.filter(c => c.status === "Manual Review").length, rejected: candidates.filter(c => c.status === "Rejected").length, avgScore: Math.round(candidates.reduce((a, c) => a + c.matchScore, 0) / candidates.length) };

  return (
    <div>
      <div style={{ marginBottom: 24 }}>
        <div style={{ fontSize: 11, letterSpacing: 4, color: "#666", fontFamily: "'Space Mono', monospace", marginBottom: 4 }}>RECRUITER PANEL</div>
        <h2 style={{ fontSize: 24, fontWeight: 700, color: "#f0f0f0", margin: 0 }}>Candidate Database</h2>
      </div>
      <div style={{ display: "grid", gridTemplateColumns: "repeat(5, 1fr)", gap: 12, marginBottom: 24 }}>
        {[["TOTAL",stats.total,"#f0f0f0"],["SHORTLISTED",stats.shortlisted,"#00e676"],["REVIEW",stats.review,"#ffd600"],["REJECTED",stats.rejected,"#ff5252"],["AVG SCORE",stats.avgScore,"#40c4ff"]].map(([label,val,color]) => (
          <div key={label} style={{ background: "#111", border: "1px solid #222", borderRadius: 8, padding: "14px 12px", textAlign: "center" }}>
            <div style={{ fontSize: 22, fontWeight: 700, color, fontFamily: "'Space Mono', monospace" }}>{val}</div>
            <div style={{ fontSize: 10, color: "#555", letterSpacing: 1, marginTop: 2, fontFamily: "'Space Mono', monospace" }}>{label}</div>
          </div>
        ))}
      </div>
      <div style={{ display: "flex", gap: 12, marginBottom: 20, alignItems: "center", flexWrap: "wrap" }}>
        <input placeholder="Search name, role..." value={search} onChange={e => setSearch(e.target.value)}
          style={{ flex: 1, minWidth: 200, background: "#111", border: "1px solid #222", borderRadius: 6, padding: "8px 14px", color: "#f0f0f0", fontSize: 13, outline: "none", fontFamily: "inherit" }} />
        <div style={{ display: "flex", gap: 6 }}>
          {statuses.map(s => (
            <button key={s} onClick={() => setFilter(s)} style={{ padding: "7px 14px", borderRadius: 6, border: `1px solid ${filter === s ? "#00e676" : "#2a2a2a"}`, background: filter === s ? "#0d2a1f" : "transparent", color: filter === s ? "#00e676" : "#666", fontSize: 11, cursor: "pointer", fontFamily: "'Space Mono', monospace" }}>{s}</button>
          ))}
        </div>
      </div>
      <div style={{ background: "#111", border: "1px solid #1a1a1a", borderRadius: 10, overflow: "hidden" }}>
        <div style={{ display: "grid", gridTemplateColumns: "60px 1fr 1fr 120px 80px 110px", padding: "10px 16px", borderBottom: "1px solid #1a1a1a", fontSize: 10, color: "#555", fontFamily: "'Space Mono', monospace", letterSpacing: 1 }}>
          {["ID","CANDIDATE","ROLE / CITY","SKILLS","SCORE","STATUS"].map(h => <div key={h}>{h}</div>)}
        </div>
        {filtered.map((c, i) => (
          <div key={c.id} onClick={() => { setSelected(c); setEditNote(c.notes || ""); }}
            style={{ display: "grid", gridTemplateColumns: "60px 1fr 1fr 120px 80px 110px", padding: "14px 16px", borderBottom: i < filtered.length - 1 ? "1px solid #1a1a1a" : "none", cursor: "pointer", alignItems: "center" }}
            onMouseEnter={e => e.currentTarget.style.background = "#161616"} onMouseLeave={e => e.currentTarget.style.background = "transparent"}>
            <div style={{ fontSize: 11, color: "#555", fontFamily: "'Space Mono', monospace" }}>{c.id}</div>
            <div><div style={{ fontWeight: 600, color: "#f0f0f0", fontSize: 14 }}>{c.name}</div><div style={{ fontSize: 11, color: "#666" }}>{c.email}</div></div>
            <div><div style={{ color: "#ccc", fontSize: 13 }}>{c.role}</div><div style={{ fontSize: 11, color: "#555" }}>📍 {c.city}</div></div>
            <div style={{ display: "flex", flexWrap: "wrap", gap: 3 }}>
              {c.skills.slice(0, 2).map(s => (<span key={s} style={{ background: "#1a2a1a", color: "#00e676", padding: "2px 6px", borderRadius: 3, fontSize: 10, fontFamily: "'Space Mono', monospace" }}>{s}</span>))}
              {c.skills.length > 2 && <span style={{ color: "#555", fontSize: 10 }}>+{c.skills.length - 2}</span>}
            </div>
            <div style={{ display: "flex", justifyContent: "center" }}><ScoreBadge score={c.matchScore} /></div>
            <StatusPill status={c.status} />
          </div>
        ))}
      </div>
      {selected && (
        <div style={{ position: "fixed", right: 0, top: 0, width: 380, height: "100vh", background: "#0e0e0e", borderLeft: "1px solid #222", padding: 24, overflowY: "auto", zIndex: 100 }}>
          <button onClick={() => setSelected(null)} style={{ position: "absolute", top: 16, right: 16, background: "none", border: "1px solid #333", color: "#888", borderRadius: 6, padding: "6px 12px", cursor: "pointer" }}>✕</button>
          <div style={{ marginBottom: 20, paddingTop: 8 }}>
            <div style={{ fontSize: 20, fontWeight: 700, color: "#f0f0f0" }}>{selected.name}</div>
            <div style={{ color: "#666", fontSize: 13, marginTop: 2 }}>{selected.role}</div>
            <div style={{ marginTop: 12 }}><StatusPill status={selected.status} /></div>
          </div>
          <div style={{ marginBottom: 16 }}>
            <div style={{ fontSize: 10, color: "#555", fontFamily: "'Space Mono', monospace", letterSpacing: 1, marginBottom: 8 }}>AI SUMMARY</div>
            <div style={{ background: "#141414", border: "1px solid #222", borderRadius: 6, padding: 12, fontSize: 13, color: "#aaa", lineHeight: 1.6 }}>{selected.summary}</div>
          </div>
          <div style={{ display: "flex", gap: 6, marginBottom: 16 }}>
            {["Shortlisted","Manual Review","Rejected"].map(s => (
              <button key={s} onClick={() => { const u = {...selected, status: s}; setSelected(u); setCandidates(prev => prev.map(c => c.id === selected.id ? u : c)); }}
                style={{ flex: 1, padding: "8px 4px", background: selected.status === s ? STATUS_CONFIG[s].bg : "transparent", border: `1px solid ${STATUS_CONFIG[s].border}`, color: STATUS_CONFIG[s].text, borderRadius: 6, cursor: "pointer", fontSize: 10, fontFamily: "'Space Mono', monospace" }}>
                {s === "Shortlisted" ? "✓" : s === "Rejected" ? "✕" : "?"} {s.split(" ")[0]}
              </button>
            ))}
          </div>
          <textarea value={editNote} onChange={e => setEditNote(e.target.value)} placeholder="Add notes..."
            style={{ width: "100%", background: "#141414", border: "1px solid #222", borderRadius: 6, padding: 10, color: "#ccc", fontSize: 13, resize: "vertical", minHeight: 80, fontFamily: "inherit", outline: "none", boxSizing: "border-box" }} />
          <button onClick={() => { const u = {...selected, notes: editNote}; setSelected(u); setCandidates(prev => prev.map(c => c.id === selected.id ? u : c)); }}
            style={{ marginTop: 8, width: "100%", padding: 10, background: "#1a2a1a", border: "1px solid #2a4a2a", color: "#00e676", borderRadius: 6, cursor: "pointer", fontFamily: "'Space Mono', monospace", fontSize: 12 }}>
            SAVE NOTES
          </button>
        </div>
      )}
    </div>
  );
}

export default function App() {
  const [view, setView] = useState("upload");
  const [pendingCandidate, setPendingCandidate] = useState(null);
  const [candidates, setCandidates] = useState(SAMPLE_CANDIDATES);

  return (
    <div style={{ minHeight: "100vh", background: "#0a0a0a", color: "#f0f0f0", fontFamily: "'Syne', system-ui, sans-serif" }}>
      <style>{`@import url('https://fonts.googleapis.com/css2?family=Syne:wght@400;600;700;800&family=Space+Mono:wght@400;700&display=swap'); * { box-sizing: border-box; }`}</style>
      <div style={{ borderBottom: "1px solid #1a1a1a", padding: "0 32px", display: "flex", alignItems: "center", justifyContent: "space-between", height: 56 }}>
        <div style={{ display: "flex", alignItems: "center", gap: 12 }}>
          <div style={{ width: 28, height: 28, background: "#00e676", borderRadius: 6, display: "flex", alignItems: "center", justifyContent: "center", fontSize: 14, fontWeight: 700, color: "#0a0a0a" }}>H</div>
          <span style={{ fontWeight: 700, fontSize: 16 }}>HireAI</span>
        </div>
        <div style={{ display: "flex", gap: 4 }}>
          {[["upload","Apply"],["dashboard","Dashboard"]].map(([v,label]) => (
            <button key={v} onClick={() => setView(v)} style={{ padding: "6px 16px", borderRadius: 6, border: `1px solid ${view === v ? "#00e676" : "transparent"}`, background: view === v ? "#0d2a1f" : "transparent", color: view === v ? "#00e676" : "#666", fontSize: 12, cursor: "pointer", fontFamily: "'Space Mono', monospace" }}>{label}</button>
          ))}
        </div>
      </div>
      <div style={{ padding: "48px 32px", maxWidth: view === "dashboard" ? 1100 : 700, margin: "0 auto" }}>
        {view === "upload" && <UploadForm onSubmit={d => { setPendingCandidate(d); setView("parsing"); }} />}
        {view === "parsing" && pendingCandidate && <ParsingView candidate={pendingCandidate} onComplete={c => { setCandidates(prev => [c, ...prev]); setView("dashboard"); }} />}
        {view === "dashboard" && <Dashboard candidates={candidates} setCandidates={setCandidates} />}
      </div>
    </div>
  );
}
