import { useState, useEffect, useRef } from "react";

const C = {
  bg: "#FAFAFA", ivory: "#F8F6F2", blue: "#2F5EFF",
  blueDark: "#1A3FCC", blueLight: "#EEF2FF", blueGlow: "#2F5EFF22",
  gold: "#D6B46A", goldLight: "#FBF5E6",
  charcoal: "#222222", muted: "#6B6B6B", light: "#999999",
  border: "#E8E4DE", borderLight: "#F0EDE8", white: "#FFFFFF",
};

const SYSTEM_PROMPT = `You are Buddy, the official AI Help Desk Assistant for the School of Computing at MIT Vishwaprayag University. Be friendly, professional, and concise.

PROGRAMS:
- B.Tech CSE: 4 years, ₹1,75,000/year
- B.Tech CSE (AI & ML): 4 years, ₹1,75,000/year
- BCA: 3 years, ₹90,000/year
- MCA: 2 years, ₹1,10,000/year

SCHOLARSHIPS:
- Scholarship I: 100% Tuition Fee Waiver — Top 3% of students
- Scholarship II: 50% Tuition Fee Waiver — Top 8% of students
- Scholarship III: 25% Tuition Fee Waiver — Top 12% of students

ATTENDANCE: Minimum 75% required.

ADMISSION (12 steps): Visit portal → Fill form → Pay ₹1000 application fee → Get Login ID & Password → Complete application → Personal interview (if applicable) → Selection via MHT-CET/JEE → Check result → Receive offer letter → Pay first installment → Document verification → Receive PRN number.

CAMPUS FACILITIES: Modern classrooms, Computer labs, Central library, Digital learning resources, Hostel, Sports areas, Cafeteria, Student amenities, Industry projects, Dedicated School of Computing.

RULES: Give brief answers first. Offer more detail if asked. Only use knowledge above. If unavailable: "I currently do not have official information for that. Please contact the university office."`;

const PROGRAMS_DATA = [
  { name: "B.Tech CSE", full: "Computer Science & Engineering", duration: "4 Years", fee: "₹1,75,000", tag: "Most Popular", color: C.blue, icon: "⌨️" },
  { name: "B.Tech CSE (AI & ML)", full: "Artificial Intelligence & Machine Learning", duration: "4 Years", fee: "₹1,75,000", tag: "Trending", color: "#7C3AED", icon: "🤖" },
  { name: "BCA", full: "Bachelor of Computer Applications", duration: "3 Years", fee: "₹90,000", tag: "UG Program", color: "#059669", icon: "🎓" },
  { name: "MCA", full: "Master of Computer Applications", duration: "2 Years", fee: "₹1,10,000", tag: "PG Program", color: C.gold, icon: "🏅" },
];

const FAQS = [
  { q: "What programs are available at School of Computing?", a: "We offer four programs: B.Tech CSE (4 yrs), B.Tech CSE with AI & ML specialization (4 yrs), BCA (3 yrs), and MCA (2 yrs)." },
  { q: "What is the B.Tech annual fee?", a: "B.Tech CSE and B.Tech CSE (AI & ML) fees are ₹1,75,000 per year. Scholarships can reduce this significantly." },
  { q: "What scholarships are available?", a: "Three scholarships: 100% fee waiver for top 3%, 50% waiver for top 8%, and 25% waiver for top 12% of students." },
  { q: "What is the minimum attendance requirement?", a: "A minimum of 75% attendance is mandatory across all programs at MIT Vishwaprayag University." },
  { q: "How does the admission process work?", a: "It's a 12-step process: apply online, pay ₹1000 fee, complete application, attend interview if required, qualify via MHT-CET or JEE, then proceed to offer letter, document verification, and receive your PRN." },
  { q: "Is hostel available on campus?", a: "Yes, hostel facilities are available on the MIT Vishwaprayag University campus." },
  { q: "Is the personal interview compulsory?", a: "Personal interviews are conducted where applicable. Not all applicants may be required to attend." },
  { q: "Can parents use Buddy?", a: "Absolutely! Buddy is designed for students, parents, and visitors. Ask anything about admissions, fees, or campus." },
  { q: "Are sports facilities available?", a: "Yes, the campus has dedicated sports areas alongside a cafeteria, library, and student amenities." },
  { q: "What is the BCA annual fee?", a: "BCA is a 3-year program with a fee of ₹90,000 per year." },
];

const QUICK_CHIPS = [
  "What programs are offered?", "Tell me about fees", "Available scholarships?",
  "How to apply?", "Attendance policy?", "Campus facilities?",
];

const FEATURES = [
  { icon: "⚡", title: "Fast Responses", desc: "Get instant answers 24/7 without waiting in queues or calling the office." },
  { icon: "🎓", title: "Student Friendly", desc: "Designed for students, parents, and visitors with simple natural language." },
  { icon: "🎙️", title: "Voice Support", desc: "Speak your question using the built-in voice input for hands-free interaction." },
  { icon: "🧠", title: "Smart Guidance", desc: "AI-powered context awareness for accurate, helpful academic guidance." },
];

const QUICK_ACCESS = [
  { icon: "📋", label: "Admissions", color: C.blue, q: "How do I apply for admission?" },
  { icon: "📚", label: "Programs", color: "#7C3AED", q: "What programs are available?" },
  { icon: "💰", label: "Fees", color: "#059669", q: "What are the program fees?" },
  { icon: "🏆", label: "Scholarships", color: C.gold, q: "Tell me about scholarships" },
  { icon: "📅", label: "Attendance", color: "#DC2626", q: "What is the attendance requirement?" },
  { icon: "🏛️", label: "Campus", color: "#0891B2", q: "Tell me about campus facilities" },
];

function formatMsg(text) {
  return text
    .replace(/\*\*(.*?)\*\*/g, '<strong>$1</strong>')
    .replace(/\n•\s/g, '\n<span style="color:#2F5EFF;font-weight:600">•</span> ')
    .replace(/\n-\s/g, '\n<span style="color:#2F5EFF;font-weight:600">•</span> ')
    .replace(/\n/g, '<br/>');
}

function TypingDots() {
  return (
    <div style={{ display: "flex", gap: 4, padding: "4px 0", alignItems: "center" }}>
      {[0, 1, 2].map(i => (
        <div key={i} style={{
          width: 7, height: 7, borderRadius: "50%", background: C.blue,
          animation: "typingPulse 1.2s ease-in-out infinite",
          animationDelay: `${i * 0.2}s`,
          opacity: 0.6,
        }} />
      ))}
    </div>
  );
}

function ChatBubble({ msg }) {
  const isUser = msg.role === "user";
  return (
    <div style={{
      display: "flex", justifyContent: isUser ? "flex-end" : "flex-start",
      marginBottom: 16, animation: "fadeUp 0.3s ease-out",
    }}>
      {!isUser && (
        <div style={{
          width: 34, height: 34, borderRadius: 10, flexShrink: 0, marginRight: 10, marginTop: 2,
          background: `linear-gradient(135deg, ${C.blue}, #5B7FFF)`,
          display: "flex", alignItems: "center", justifyContent: "center",
          color: "white", fontFamily: "DM Serif Display, serif", fontSize: 16, fontWeight: 700,
          boxShadow: `0 4px 12px ${C.blueGlow}`,
        }}>B</div>
      )}
      <div style={{
        maxWidth: "74%",
        background: isUser ? `linear-gradient(135deg, ${C.blue}, #4F75FF)` : C.white,
        color: isUser ? "white" : C.charcoal,
        borderRadius: isUser ? "18px 4px 18px 18px" : "4px 18px 18px 18px",
        padding: "12px 16px",
        fontFamily: "Outfit, sans-serif", fontSize: 14, lineHeight: 1.6,
        boxShadow: isUser ? `0 4px 16px ${C.blue}30` : "0 2px 12px rgba(0,0,0,0.06)",
        border: isUser ? "none" : `1px solid ${C.borderLight}`,
      }}>
        {isUser
          ? <span>{msg.content}</span>
          : <span dangerouslySetInnerHTML={{ __html: formatMsg(msg.content) }} />
        }
      </div>
    </div>
  );
}

export default function BuddyApp() {
  const [page, setPage] = useState("home");
  const [messages, setMessages] = useState([{
    role: "assistant",
    content: `Hello! 👋 I'm **Buddy**, your AI Help Desk Assistant for the School of Computing at MIT Vishwaprayag University.\n\nI can help you with:\n• Admissions & Application\n• Programs & Duration\n• Fees & Scholarships\n• Attendance Policy\n• Campus Facilities\n\nWhat would you like to know?`,
  }]);
  const [input, setInput] = useState("");
  const [typing, setTyping] = useState(false);
  const [faqOpen, setFaqOpen] = useState(null);
  const [listening, setListening] = useState(false);
  const chatEnd = useRef(null);

  useEffect(() => {
    const link = document.createElement("link");
    link.rel = "stylesheet";
    link.href = "https://fonts.googleapis.com/css2?family=DM+Serif+Display:ital@0;1&family=Outfit:wght@300;400;500;600;700&display=swap";
    document.head.appendChild(link);
    const style = document.createElement("style");
    style.textContent = `
      @keyframes typingPulse { 0%,80%,100%{transform:scale(1);opacity:0.5} 40%{transform:scale(1.3);opacity:1} }
      @keyframes fadeUp { from{opacity:0;transform:translateY(10px)} to{opacity:1;transform:translateY(0)} }
      @keyframes float { 0%,100%{transform:translateY(0)} 50%{transform:translateY(-10px)} }
      @keyframes shimmer { 0%{background-position:-200% 0} 100%{background-position:200% 0} }
      * { box-sizing: border-box; margin: 0; padding: 0; }
      body { background: #FAFAFA; }
      ::-webkit-scrollbar { width: 4px; } ::-webkit-scrollbar-thumb { background: #D0CBE8; border-radius: 4px; }
      button:hover { opacity: 0.88; transform: translateY(-1px); }
    `;
    document.head.appendChild(style);
  }, []);

  useEffect(() => { chatEnd.current?.scrollIntoView({ behavior: "smooth" }); }, [messages, typing]);

  const send = async (text) => {
    if (!text?.trim() || typing) return;
    const userMsg = { role: "user", content: text.trim() };
    const updated = [...messages, userMsg];
    setMessages(updated);
    setInput("");
    setTyping(true);
    try {
      const res = await fetch("https://api.anthropic.com/v1/messages", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({
          model: "claude-sonnet-4-20250514",
          max_tokens: 1000,
          system: SYSTEM_PROMPT,
          messages: updated.map(m => ({ role: m.role, content: m.content })),
        }),
      });
      const data = await res.json();
      const reply = data.content?.[0]?.text || "I'm having trouble responding right now. Please try again.";
      setMessages(prev => [...prev, { role: "assistant", content: reply }]);
    } catch {
      setMessages(prev => [...prev, { role: "assistant", content: "Connection issue. Please try again in a moment." }]);
    }
    setTyping(false);
  };

  const startVoice = () => {
    const SR = window.SpeechRecognition || window.webkitSpeechRecognition;
    if (!SR) { alert("Voice input not supported in this browser."); return; }
    const r = new SR(); r.lang = "en-IN"; r.continuous = false;
    r.onstart = () => setListening(true);
    r.onresult = e => setInput(e.results[0][0].transcript);
    r.onend = () => setListening(false);
    r.start();
  };

  // ─── NAV ───────────────────────────────────────────────────────────
  const Nav = () => (
    <nav style={{
      position: "sticky", top: 0, zIndex: 200,
      background: "rgba(250,250,250,0.94)", backdropFilter: "blur(16px)",
      borderBottom: `1px solid ${C.border}`,
    }}>
      <div style={{ maxWidth: 1200, margin: "0 auto", padding: "0 40px", height: 70, display: "flex", alignItems: "center", justifyContent: "space-between" }}>
        <div style={{ display: "flex", alignItems: "center", gap: 12, cursor: "pointer" }} onClick={() => setPage("home")}>
          <div style={{
            width: 40, height: 40, borderRadius: 11,
            background: `linear-gradient(135deg, ${C.blue} 0%, #5B7FFF 100%)`,
            display: "flex", alignItems: "center", justifyContent: "center",
            color: "white", fontFamily: "DM Serif Display, serif", fontSize: 20,
            boxShadow: `0 4px 14px ${C.blue}40`,
          }}>B</div>
          <div>
            <div style={{ fontFamily: "DM Serif Display, serif", fontSize: 20, color: C.charcoal, lineHeight: 1 }}>Buddy</div>
            <div style={{ fontFamily: "Outfit, sans-serif", fontSize: 10, color: C.muted, letterSpacing: "0.8px", textTransform: "uppercase", marginTop: 2 }}>MIT Vishwaprayag University</div>
          </div>
        </div>
        <div style={{ display: "flex", alignItems: "center", gap: 6 }}>
          {[["home", "Home"], ["chat", "Ask Buddy"], ["dashboard", "Dashboard"]].map(([p, l]) => (
            <button key={p} onClick={() => setPage(p)} style={{
              background: page === p ? C.blueLight : "none",
              border: "none", cursor: "pointer",
              fontFamily: "Outfit, sans-serif", fontWeight: 600, fontSize: 13.5,
              color: page === p ? C.blue : C.muted,
              borderRadius: 8, padding: "7px 16px",
              transition: "all 0.18s",
            }}>{l}</button>
          ))}
          <button onClick={() => setPage("chat")} style={{
            marginLeft: 8, background: C.blue, color: "white", border: "none",
            borderRadius: 9, padding: "10px 22px",
            fontFamily: "Outfit, sans-serif", fontWeight: 600, fontSize: 14,
            cursor: "pointer", boxShadow: `0 6px 20px ${C.blue}35`, transition: "all 0.18s",
          }}>Chat Now</button>
        </div>
      </div>
    </nav>
  );

  // ─── HERO ──────────────────────────────────────────────────────────
  const Hero = () => (
    <section style={{ background: C.bg, padding: "90px 40px 80px", position: "relative", overflow: "hidden" }}>
      <div style={{ position: "absolute", top: -120, right: -120, width: 500, height: 500, borderRadius: "50%", background: `radial-gradient(circle, ${C.blueLight} 0%, transparent 65%)` }} />
      <div style={{ position: "absolute", bottom: -80, left: -80, width: 300, height: 300, borderRadius: "50%", background: `radial-gradient(circle, ${C.goldLight} 0%, transparent 70%)`, opacity: 0.7 }} />
      <div style={{ maxWidth: 1200, margin: "0 auto", display: "grid", gridTemplateColumns: "1fr 1fr", gap: 80, alignItems: "center", position: "relative" }}>
        <div>
          <div style={{ display: "inline-flex", alignItems: "center", gap: 8, background: C.blueLight, borderRadius: 20, padding: "6px 14px", marginBottom: 28 }}>
            <div style={{ width: 7, height: 7, borderRadius: "50%", background: C.blue, animation: "typingPulse 2s infinite" }} />
            <span style={{ fontFamily: "Outfit, sans-serif", fontSize: 11.5, color: C.blue, fontWeight: 600, letterSpacing: "0.5px", textTransform: "uppercase" }}>AI-Powered Help Desk · School of Computing</span>
          </div>
          <h1 style={{ fontFamily: "DM Serif Display, serif", fontSize: "clamp(44px, 5vw, 68px)", color: C.charcoal, lineHeight: 1.05, marginBottom: 18 }}>
            Welcome to<br /><span style={{ color: C.blue }}>Buddy</span>
          </h1>
          <p style={{ fontFamily: "Outfit, sans-serif", fontWeight: 500, fontSize: 19, color: C.muted, marginBottom: 12 }}>Your Smart Campus Companion</p>
          <p style={{ fontFamily: "Outfit, sans-serif", fontWeight: 400, fontSize: 15.5, color: C.muted, lineHeight: 1.75, maxWidth: 460, marginBottom: 38 }}>
            Get instant answers about admissions, courses, fees, scholarships, attendance, and campus information — available 24/7.
          </p>
          <div style={{ display: "flex", gap: 14 }}>
            <button onClick={() => setPage("chat")} style={{
              background: C.blue, color: "white", border: "none", borderRadius: 12, padding: "15px 32px",
              fontFamily: "Outfit, sans-serif", fontWeight: 700, fontSize: 15,
              cursor: "pointer", boxShadow: `0 8px 28px ${C.blue}45`, transition: "all 0.2s",
            }}>Ask Buddy →</button>
            <button onClick={() => document.getElementById("programs")?.scrollIntoView({ behavior: "smooth" })} style={{
              background: "transparent", color: C.charcoal, border: `1.5px solid ${C.border}`,
              borderRadius: 12, padding: "15px 32px",
              fontFamily: "Outfit, sans-serif", fontWeight: 600, fontSize: 15,
              cursor: "pointer", transition: "all 0.2s",
            }}>Explore Programs</button>
          </div>
        </div>
        {/* Hero Visual */}
        <div style={{ display: "flex", justifyContent: "center", alignItems: "center" }}>
          <div style={{ position: "relative", width: 380, height: 380 }}>
            {/* Outer ring */}
            <div style={{ position: "absolute", inset: 0, borderRadius: "50%", border: `1.5px dashed ${C.border}`, opacity: 0.6 }} />
            <div style={{ position: "absolute", inset: 30, borderRadius: "50%", border: `1.5px dashed ${C.blueLight}` }} />
            {/* Center card */}
            <div style={{
              position: "absolute", top: "50%", left: "50%", transform: "translate(-50%,-50%)",
              background: C.white, borderRadius: 20, padding: "24px 28px", width: 240,
              boxShadow: "0 20px 60px rgba(0,0,0,0.1)", border: `1px solid ${C.borderLight}`,
              animation: "float 4s ease-in-out infinite",
            }}>
              <div style={{ display: "flex", alignItems: "center", gap: 10, marginBottom: 16 }}>
                <div style={{ width: 36, height: 36, borderRadius: 9, background: `linear-gradient(135deg, ${C.blue}, #5B7FFF)`, display: "flex", alignItems: "center", justifyContent: "center", color: "white", fontFamily: "DM Serif Display, serif", fontSize: 18 }}>B</div>
                <div>
                  <div style={{ fontFamily: "Outfit, sans-serif", fontWeight: 700, fontSize: 13, color: C.charcoal }}>Buddy</div>
                  <div style={{ fontFamily: "Outfit, sans-serif", fontSize: 10, color: "#22C55E", fontWeight: 600 }}>● Online</div>
                </div>
              </div>
              <div style={{ background: C.blueLight, borderRadius: 10, padding: "8px 12px", marginBottom: 8 }}>
                <p style={{ fontFamily: "Outfit, sans-serif", fontSize: 12, color: C.blue, fontWeight: 500 }}>What is the B.Tech fee?</p>
              </div>
              <div style={{ background: C.ivory, borderRadius: 10, padding: "8px 12px" }}>
                <p style={{ fontFamily: "Outfit, sans-serif", fontSize: 12, color: C.charcoal, lineHeight: 1.5 }}>B.Tech CSE fee is <strong>₹1,75,000/year</strong>. Scholarships available! 🎓</p>
              </div>
              <div style={{ display: "flex", gap: 4, marginTop: 12 }}>
                {[0, 1, 2].map(i => <div key={i} style={{ width: 7, height: 7, borderRadius: "50%", background: C.blue, animation: "typingPulse 1.2s infinite", animationDelay: `${i * 0.2}s`, opacity: 0.5 }} />)}
              </div>
            </div>
            {/* Floating chips */}
            {[
              { top: 20, left: 20, text: "📋 Admissions", bg: C.blueLight, color: C.blue },
              { top: 20, right: 20, text: "🏆 Scholarships", bg: C.goldLight, color: "#A0811A" },
              { bottom: 20, left: 20, text: "🎓 4 Programs", bg: "#F0FDF4", color: "#059669" },
              { bottom: 20, right: 20, text: "🏛️ Campus", bg: "#FEF3C7", color: "#D97706" },
            ].map((c, i) => (
              <div key={i} style={{
                position: "absolute", ...c,
                background: c.bg, color: c.color, borderRadius: 20, padding: "6px 12px",
                fontFamily: "Outfit, sans-serif", fontSize: 11, fontWeight: 600,
                boxShadow: "0 4px 14px rgba(0,0,0,0.07)",
                animation: `float ${3.5 + i * 0.5}s ease-in-out infinite`, animationDelay: `${i * 0.5}s`,
              }}>{c.text}</div>
            ))}
          </div>
        </div>
      </div>
    </section>
  );

  // ─── QUICK ACCESS ──────────────────────────────────────────────────
  const QuickAccess = () => (
    <section style={{ background: C.ivory, padding: "72px 40px" }}>
      <div style={{ maxWidth: 1200, margin: "0 auto" }}>
        <div style={{ textAlign: "center", marginBottom: 48 }}>
          <h2 style={{ fontFamily: "DM Serif Display, serif", fontSize: 38, color: C.charcoal, marginBottom: 10 }}>Quick Access</h2>
          <p style={{ fontFamily: "Outfit, sans-serif", fontSize: 16, color: C.muted }}>Jump to the information you need instantly</p>
        </div>
        <div style={{ display: "grid", gridTemplateColumns: "repeat(6, 1fr)", gap: 16 }}>
          {QUICK_ACCESS.map(({ icon, label, color, q }) => (
            <button key={label} onClick={() => { setPage("chat"); setTimeout(() => send(q), 400); }} style={{
              background: C.white, border: `1.5px solid ${C.borderLight}`, borderRadius: 16,
              padding: "28px 12px", cursor: "pointer", transition: "all 0.22s",
              boxShadow: "0 2px 12px rgba(0,0,0,0.04)",
              display: "flex", flexDirection: "column", alignItems: "center", gap: 10,
            }}
              onMouseEnter={e => { e.currentTarget.style.borderColor = color; e.currentTarget.style.transform = "translateY(-4px)"; e.currentTarget.style.boxShadow = `0 8px 24px ${color}20`; }}
              onMouseLeave={e => { e.currentTarget.style.borderColor = C.borderLight; e.currentTarget.style.transform = "none"; e.currentTarget.style.boxShadow = "0 2px 12px rgba(0,0,0,0.04)"; }}>
              <div style={{ width: 48, height: 48, borderRadius: 13, background: color + "18", display: "flex", alignItems: "center", justifyContent: "center", fontSize: 22 }}>{icon}</div>
              <span style={{ fontFamily: "Outfit, sans-serif", fontWeight: 600, fontSize: 13, color: C.charcoal }}>{label}</span>
            </button>
          ))}
        </div>
      </div>
    </section>
  );

  // ─── CHAT PREVIEW ─────────────────────────────────────────────────
  const ChatPreview = () => (
    <section style={{ background: C.bg, padding: "80px 40px" }}>
      <div style={{ maxWidth: 1200, margin: "0 auto", display: "grid", gridTemplateColumns: "1fr 1fr", gap: 80, alignItems: "center" }}>
        <div>
          <div style={{ display: "inline-block", background: C.blueLight, borderRadius: 20, padding: "5px 14px", marginBottom: 20 }}>
            <span style={{ fontFamily: "Outfit, sans-serif", fontSize: 11.5, color: C.blue, fontWeight: 600, letterSpacing: "0.5px", textTransform: "uppercase" }}>AI Chat Experience</span>
          </div>
          <h2 style={{ fontFamily: "DM Serif Display, serif", fontSize: 40, color: C.charcoal, lineHeight: 1.1, marginBottom: 18 }}>Ask anything, get answers <em>instantly</em></h2>
          <p style={{ fontFamily: "Outfit, sans-serif", fontSize: 16, color: C.muted, lineHeight: 1.75, marginBottom: 32 }}>
            Buddy understands natural language. No forms, no wait times — just ask in plain English and get clear, accurate answers about MIT Vishwaprayag University.
          </p>
          <div style={{ display: "flex", flexDirection: "column", gap: 12 }}>
            {[["🧠", "Understands context and follow-up questions"], ["🎙️", "Voice input for hands-free queries"], ["📱", "Optimized for mobile and desktop"]].map(([ic, text]) => (
              <div key={text} style={{ display: "flex", alignItems: "center", gap: 12 }}>
                <div style={{ width: 36, height: 36, background: C.blueLight, borderRadius: 9, display: "flex", alignItems: "center", justifyContent: "center", fontSize: 18, flexShrink: 0 }}>{ic}</div>
                <span style={{ fontFamily: "Outfit, sans-serif", fontSize: 14.5, color: C.muted, fontWeight: 500 }}>{text}</span>
              </div>
            ))}
          </div>
          <button onClick={() => setPage("chat")} style={{
            marginTop: 32, background: C.blue, color: "white", border: "none", borderRadius: 11, padding: "13px 28px",
            fontFamily: "Outfit, sans-serif", fontWeight: 700, fontSize: 14.5,
            cursor: "pointer", boxShadow: `0 6px 22px ${C.blue}40`, transition: "all 0.2s",
          }}>Try Buddy Now →</button>
        </div>
        {/* Mock Chat UI */}
        <div style={{ background: C.white, borderRadius: 20, boxShadow: "0 24px 64px rgba(0,0,0,0.09)", border: `1px solid ${C.borderLight}`, overflow: "hidden" }}>
          <div style={{ background: `linear-gradient(135deg, ${C.blue}, #4F75FF)`, padding: "16px 22px", display: "flex", alignItems: "center", gap: 12 }}>
            <div style={{ width: 36, height: 36, borderRadius: 9, background: "rgba(255,255,255,0.2)", display: "flex", alignItems: "center", justifyContent: "center", color: "white", fontFamily: "DM Serif Display, serif", fontSize: 18 }}>B</div>
            <div>
              <div style={{ color: "white", fontFamily: "Outfit, sans-serif", fontWeight: 700, fontSize: 14 }}>Buddy</div>
              <div style={{ color: "rgba(255,255,255,0.75)", fontFamily: "Outfit, sans-serif", fontSize: 11 }}>● Always Available</div>
            </div>
          </div>
          <div style={{ padding: "20px 18px", display: "flex", flexDirection: "column", gap: 12, background: "#F9FAFE", minHeight: 260 }}>
            {[
              { r: false, t: "Hello! 👋 I'm Buddy. How can I help you today?" },
              { r: true, t: "What scholarships are available?" },
              { r: false, t: "We offer 3 scholarships:\n• **100% waiver** — Top 3% students\n• **50% waiver** — Top 8% students\n• **25% waiver** — Top 12% students" },
            ].map((m, i) => (
              <div key={i} style={{ display: "flex", justifyContent: m.r ? "flex-end" : "flex-start" }}>
                <div style={{
                  maxWidth: "78%", background: m.r ? `linear-gradient(135deg, ${C.blue}, #4F75FF)` : C.white,
                  color: m.r ? "white" : C.charcoal, borderRadius: m.r ? "14px 4px 14px 14px" : "4px 14px 14px 14px",
                  padding: "10px 14px", fontFamily: "Outfit, sans-serif", fontSize: 13, lineHeight: 1.55,
                  boxShadow: m.r ? `0 4px 12px ${C.blue}25` : "0 2px 8px rgba(0,0,0,0.06)",
                }}>
                  <span dangerouslySetInnerHTML={{ __html: formatMsg(m.t) }} />
                </div>
              </div>
            ))}
            <div style={{ display: "flex", justifyContent: "flex-start" }}>
              <div style={{ background: C.white, borderRadius: "4px 14px 14px 14px", padding: "10px 14px", boxShadow: "0 2px 8px rgba(0,0,0,0.06)" }}>
                <TypingDots />
              </div>
            </div>
          </div>
          <div style={{ padding: "14px 18px", borderTop: `1px solid ${C.borderLight}`, display: "flex", gap: 8 }}>
            <div style={{ flex: 1, background: C.ivory, borderRadius: 10, padding: "10px 14px", fontFamily: "Outfit, sans-serif", fontSize: 13, color: C.light }}>Ask a question...</div>
            <button onClick={() => setPage("chat")} style={{ background: C.blue, color: "white", border: "none", borderRadius: 10, padding: "0 16px", cursor: "pointer", fontFamily: "Outfit, sans-serif", fontWeight: 700, fontSize: 13 }}>Send</button>
          </div>
        </div>
      </div>
    </section>
  );

  // ─── WHY BUDDY ─────────────────────────────────────────────────────
  const WhyBuddy = () => (
    <section style={{ background: C.ivory, padding: "80px 40px" }}>
      <div style={{ maxWidth: 1200, margin: "0 auto" }}>
        <div style={{ textAlign: "center", marginBottom: 52 }}>
          <h2 style={{ fontFamily: "DM Serif Display, serif", fontSize: 38, color: C.charcoal, marginBottom: 10 }}>Why Buddy?</h2>
          <p style={{ fontFamily: "Outfit, sans-serif", fontSize: 16, color: C.muted }}>Built for the modern university experience</p>
        </div>
        <div style={{ display: "grid", gridTemplateColumns: "repeat(4, 1fr)", gap: 24 }}>
          {FEATURES.map(({ icon, title, desc }) => (
            <div key={title} style={{
              background: C.white, borderRadius: 18, padding: "32px 24px",
              border: `1px solid ${C.borderLight}`, boxShadow: "0 2px 16px rgba(0,0,0,0.04)",
              transition: "all 0.22s",
            }}
              onMouseEnter={e => { e.currentTarget.style.transform = "translateY(-4px)"; e.currentTarget.style.boxShadow = `0 12px 36px rgba(0,0,0,0.08)`; }}
              onMouseLeave={e => { e.currentTarget.style.transform = "none"; e.currentTarget.style.boxShadow = "0 2px 16px rgba(0,0,0,0.04)"; }}>
              <div style={{ fontSize: 36, marginBottom: 18 }}>{icon}</div>
              <h3 style={{ fontFamily: "DM Serif Display, serif", fontSize: 22, color: C.charcoal, marginBottom: 10 }}>{title}</h3>
              <p style={{ fontFamily: "Outfit, sans-serif", fontSize: 14, color: C.muted, lineHeight: 1.7 }}>{desc}</p>
            </div>
          ))}
        </div>
      </div>
    </section>
  );

  // ─── PROGRAMS ─────────────────────────────────────────────────────
  const ProgramsSection = () => (
    <section id="programs" style={{ background: C.bg, padding: "80px 40px" }}>
      <div style={{ maxWidth: 1200, margin: "0 auto" }}>
        <div style={{ textAlign: "center", marginBottom: 52 }}>
          <h2 style={{ fontFamily: "DM Serif Display, serif", fontSize: 38, color: C.charcoal, marginBottom: 10 }}>Programs Offered</h2>
          <p style={{ fontFamily: "Outfit, sans-serif", fontSize: 16, color: C.muted }}>School of Computing · MIT Vishwaprayag University</p>
        </div>
        <div style={{ display: "grid", gridTemplateColumns: "repeat(4, 1fr)", gap: 20 }}>
          {PROGRAMS_DATA.map(({ name, full, duration, fee, tag, color, icon }) => (
            <div key={name} style={{
              background: C.white, borderRadius: 18, overflow: "hidden",
              border: `1px solid ${C.borderLight}`, boxShadow: "0 2px 16px rgba(0,0,0,0.04)",
              transition: "all 0.22s",
            }}
              onMouseEnter={e => { e.currentTarget.style.transform = "translateY(-4px)"; e.currentTarget.style.boxShadow = `0 16px 44px ${color}18`; e.currentTarget.style.borderColor = color + "50"; }}
              onMouseLeave={e => { e.currentTarget.style.transform = "none"; e.currentTarget.style.boxShadow = "0 2px 16px rgba(0,0,0,0.04)"; e.currentTarget.style.borderColor = C.borderLight; }}>
              <div style={{ background: color + "12", padding: "24px 22px 18px", borderBottom: `1px solid ${color}20` }}>
                <div style={{ fontSize: 32, marginBottom: 10 }}>{icon}</div>
                {tag && <span style={{ background: color, color: "white", borderRadius: 6, padding: "3px 10px", fontSize: 10, fontFamily: "Outfit, sans-serif", fontWeight: 700, letterSpacing: "0.4px", textTransform: "uppercase" }}>{tag}</span>}
              </div>
              <div style={{ padding: "18px 22px 24px" }}>
                <h3 style={{ fontFamily: "DM Serif Display, serif", fontSize: 20, color: C.charcoal, marginBottom: 4 }}>{name}</h3>
                <p style={{ fontFamily: "Outfit, sans-serif", fontSize: 12.5, color: C.muted, marginBottom: 18, lineHeight: 1.4 }}>{full}</p>
                <div style={{ display: "flex", flexDirection: "column", gap: 8 }}>
                  {[["🕐", "Duration", duration], ["💰", "Annual Fee", fee + "/yr"]].map(([ic, lbl, val]) => (
                    <div key={lbl} style={{ display: "flex", justifyContent: "space-between", alignItems: "center" }}>
                      <span style={{ fontFamily: "Outfit, sans-serif", fontSize: 12, color: C.muted }}>{ic} {lbl}</span>
                      <span style={{ fontFamily: "Outfit, sans-serif", fontSize: 13, fontWeight: 700, color: C.charcoal }}>{val}</span>
                    </div>
                  ))}
                </div>
                <button onClick={() => { setPage("chat"); setTimeout(() => send(`Tell me about ${name} program`), 400); }} style={{
                  marginTop: 18, width: "100%", background: color + "12", color: color,
                  border: `1.5px solid ${color}30`, borderRadius: 9, padding: "10px",
                  fontFamily: "Outfit, sans-serif", fontWeight: 700, fontSize: 13,
                  cursor: "pointer", transition: "all 0.2s",
                }}>Learn More →</button>
              </div>
            </div>
          ))}
        </div>
        {/* Scholarships */}
        <div style={{ marginTop: 60, background: `linear-gradient(135deg, ${C.blue}06, ${C.goldLight})`, borderRadius: 20, padding: "40px", border: `1px solid ${C.border}` }}>
          <h3 style={{ fontFamily: "DM Serif Display, serif", fontSize: 30, color: C.charcoal, marginBottom: 8, textAlign: "center" }}>Scholarships Available</h3>
          <p style={{ textAlign: "center", fontFamily: "Outfit, sans-serif", fontSize: 15, color: C.muted, marginBottom: 32 }}>Merit-based tuition fee waivers for deserving students</p>
          <div style={{ display: "grid", gridTemplateColumns: "repeat(3, 1fr)", gap: 20 }}>
            {[
              { name: "Scholarship I", benefit: "100%", desc: "Full Tuition Fee Waiver", criteria: "Top 3% of students", color: "#FFD700" },
              { name: "Scholarship II", benefit: "50%", desc: "Half Tuition Fee Waiver", criteria: "Top 8% of students", color: C.gold },
              { name: "Scholarship III", benefit: "25%", desc: "Quarter Tuition Fee Waiver", criteria: "Top 12% of students", color: "#C4A35A" },
            ].map(({ name, benefit, desc, criteria, color }) => (
              <div key={name} style={{ background: C.white, borderRadius: 16, padding: "28px 24px", border: `1px solid ${color}40`, boxShadow: "0 2px 16px rgba(0,0,0,0.04)", textAlign: "center" }}>
                <div style={{ fontFamily: "DM Serif Display, serif", fontSize: 44, color: color, lineHeight: 1 }}>{benefit}</div>
                <div style={{ fontFamily: "Outfit, sans-serif", fontWeight: 700, fontSize: 13, color: C.charcoal, marginTop: 6, marginBottom: 4 }}>{name}</div>
                <div style={{ fontFamily: "Outfit, sans-serif", fontSize: 12, color: C.muted, marginBottom: 12 }}>{desc}</div>
                <div style={{ background: color + "18", borderRadius: 8, padding: "6px 12px", display: "inline-block" }}>
                  <span style={{ fontFamily: "Outfit, sans-serif", fontSize: 11.5, fontWeight: 600, color: color === "#FFD700" ? "#A0811A" : color }}>{criteria}</span>
                </div>
              </div>
            ))}
          </div>
        </div>
      </div>
    </section>
  );

  // ─── FAQ ──────────────────────────────────────────────────────────
  const FAQSection = () => (
    <section style={{ background: C.ivory, padding: "80px 40px" }}>
      <div style={{ maxWidth: 760, margin: "0 auto" }}>
        <div style={{ textAlign: "center", marginBottom: 52 }}>
          <h2 style={{ fontFamily: "DM Serif Display, serif", fontSize: 38, color: C.charcoal, marginBottom: 10 }}>Frequently Asked Questions</h2>
          <p style={{ fontFamily: "Outfit, sans-serif", fontSize: 16, color: C.muted }}>Quick answers to common questions</p>
        </div>
        <div style={{ display: "flex", flexDirection: "column", gap: 10 }}>
          {FAQS.map(({ q, a }, i) => (
            <div key={i} style={{
              background: C.white, borderRadius: 14, border: `1px solid ${faqOpen === i ? C.blue + "40" : C.borderLight}`,
              overflow: "hidden", transition: "all 0.2s", boxShadow: faqOpen === i ? `0 4px 20px ${C.blue}10` : "none",
            }}>
              <button onClick={() => setFaqOpen(faqOpen === i ? null : i)} style={{
                width: "100%", background: "none", border: "none", cursor: "pointer",
                padding: "18px 22px", display: "flex", justifyContent: "space-between", alignItems: "center", gap: 16,
              }}>
                <span style={{ fontFamily: "Outfit, sans-serif", fontWeight: 600, fontSize: 14.5, color: C.charcoal, textAlign: "left" }}>{q}</span>
                <span style={{ color: C.blue, fontSize: 20, flexShrink: 0, transition: "transform 0.2s", transform: faqOpen === i ? "rotate(45deg)" : "none" }}>+</span>
              </button>
              {faqOpen === i && (
                <div style={{ padding: "0 22px 18px", fontFamily: "Outfit, sans-serif", fontSize: 14, color: C.muted, lineHeight: 1.75, borderTop: `1px solid ${C.borderLight}`, paddingTop: 14 }}>
                  {a}
                </div>
              )}
            </div>
          ))}
        </div>
        <div style={{ textAlign: "center", marginTop: 40 }}>
          <p style={{ fontFamily: "Outfit, sans-serif", fontSize: 15, color: C.muted, marginBottom: 16 }}>Still have questions? Ask Buddy directly!</p>
          <button onClick={() => setPage("chat")} style={{
            background: C.blue, color: "white", border: "none", borderRadius: 11, padding: "13px 30px",
            fontFamily: "Outfit, sans-serif", fontWeight: 700, fontSize: 15,
            cursor: "pointer", boxShadow: `0 6px 22px ${C.blue}40`,
          }}>Chat with Buddy</button>
        </div>
      </div>
    </section>
  );

  // ─── FOOTER ───────────────────────────────────────────────────────
  const Footer = () => (
    <footer style={{ background: C.charcoal, padding: "60px 40px 30px" }}>
      <div style={{ maxWidth: 1200, margin: "0 auto" }}>
        <div style={{ display: "grid", gridTemplateColumns: "2fr 1fr 1fr 1fr", gap: 48, marginBottom: 48 }}>
          <div>
            <div style={{ display: "flex", alignItems: "center", gap: 10, marginBottom: 16 }}>
              <div style={{ width: 38, height: 38, borderRadius: 10, background: `linear-gradient(135deg, ${C.blue}, #5B7FFF)`, display: "flex", alignItems: "center", justifyContent: "center", color: "white", fontFamily: "DM Serif Display, serif", fontSize: 20 }}>B</div>
              <div>
                <div style={{ fontFamily: "DM Serif Display, serif", fontSize: 20, color: "white" }}>Buddy</div>
                <div style={{ fontFamily: "Outfit, sans-serif", fontSize: 10, color: "#888", letterSpacing: "0.5px", textTransform: "uppercase" }}>School of Computing</div>
              </div>
            </div>
            <p style={{ fontFamily: "Outfit, sans-serif", fontSize: 13.5, color: "#999", lineHeight: 1.8, maxWidth: 280 }}>
              AI-powered help desk for MIT Vishwaprayag University. Providing instant, accurate information to students, parents, and visitors.
            </p>
          </div>
          {[
            { head: "Programs", items: ["B.Tech CSE", "B.Tech CSE (AI&ML)", "BCA", "MCA"] },
            { head: "Quick Links", items: ["Admissions", "Scholarships", "Attendance Policy", "Campus Facilities"] },
            { head: "Contact", items: ["School of Computing", "MIT Vishwaprayag University", "admissions@mitvpu.edu.in", "+91 XXXX XXX XXX"] },
          ].map(({ head, items }) => (
            <div key={head}>
              <h4 style={{ fontFamily: "Outfit, sans-serif", fontWeight: 700, fontSize: 13, color: "white", textTransform: "uppercase", letterSpacing: "0.6px", marginBottom: 18 }}>{head}</h4>
              {items.map(item => <p key={item} style={{ fontFamily: "Outfit, sans-serif", fontSize: 13, color: "#888", marginBottom: 8, lineHeight: 1.4 }}>{item}</p>)}
            </div>
          ))}
        </div>
        <div style={{ borderTop: "1px solid #333", paddingTop: 24, display: "flex", justifyContent: "space-between", alignItems: "center" }}>
          <p style={{ fontFamily: "Outfit, sans-serif", fontSize: 12.5, color: "#666" }}>© 2025 MIT Vishwaprayag University · School of Computing. All rights reserved.</p>
          <p style={{ fontFamily: "Outfit, sans-serif", fontSize: 12.5, color: "#666" }}>Powered by <span style={{ color: C.blue }}>Buddy AI</span></p>
        </div>
      </div>
    </footer>
  );

  // ─── CHAT PAGE ─────────────────────────────────────────────────────
  const ChatPage = () => (
    <div style={{ display: "flex", height: "calc(100vh - 70px)" }}>
      {/* Sidebar */}
      <div style={{ width: 280, background: C.ivory, borderRight: `1px solid ${C.border}`, display: "flex", flexDirection: "column", padding: "24px 16px", gap: 8 }}>
        <div style={{ padding: "12px 16px", marginBottom: 8 }}>
          <h3 style={{ fontFamily: "DM Serif Display, serif", fontSize: 20, color: C.charcoal, marginBottom: 4 }}>Ask Buddy</h3>
          <p style={{ fontFamily: "Outfit, sans-serif", fontSize: 12, color: C.muted }}>School of Computing Help Desk</p>
        </div>
        <div style={{ fontFamily: "Outfit, sans-serif", fontSize: 11, fontWeight: 700, color: C.light, letterSpacing: "0.6px", textTransform: "uppercase", padding: "0 16px", marginBottom: 4 }}>Quick Questions</div>
        {QUICK_CHIPS.map(chip => (
          <button key={chip} onClick={() => send(chip)} style={{
            background: "none", border: "none", cursor: "pointer", textAlign: "left",
            fontFamily: "Outfit, sans-serif", fontSize: 13, color: C.muted, fontWeight: 500,
            padding: "10px 16px", borderRadius: 9, transition: "all 0.15s",
          }}
            onMouseEnter={e => { e.currentTarget.style.background = C.blueLight; e.currentTarget.style.color = C.blue; }}
            onMouseLeave={e => { e.currentTarget.style.background = "none"; e.currentTarget.style.color = C.muted; }}>
            {chip}
          </button>
        ))}
        <div style={{ marginTop: "auto", padding: "16px", background: C.blueLight, borderRadius: 14 }}>
          <p style={{ fontFamily: "Outfit, sans-serif", fontSize: 12, color: C.blue, fontWeight: 600, marginBottom: 4 }}>📞 Need direct help?</p>
          <p style={{ fontFamily: "Outfit, sans-serif", fontSize: 11.5, color: C.blue, lineHeight: 1.5, opacity: 0.8 }}>Contact the university office for official queries.</p>
        </div>
      </div>
      {/* Chat Area */}
      <div style={{ flex: 1, display: "flex", flexDirection: "column", background: "#F9FAFE" }}>
        {/* Chat Header */}
        <div style={{ background: C.white, borderBottom: `1px solid ${C.border}`, padding: "16px 28px", display: "flex", alignItems: "center", gap: 14 }}>
          <div style={{ width: 42, height: 42, borderRadius: 12, background: `linear-gradient(135deg, ${C.blue}, #5B7FFF)`, display: "flex", alignItems: "center", justifyContent: "center", color: "white", fontFamily: "DM Serif Display, serif", fontSize: 22, boxShadow: `0 4px 12px ${C.blue}40` }}>B</div>
          <div>
            <div style={{ fontFamily: "Outfit, sans-serif", fontWeight: 700, fontSize: 15.5, color: C.charcoal }}>Buddy</div>
            <div style={{ display: "flex", alignItems: "center", gap: 5 }}>
              <div style={{ width: 6, height: 6, borderRadius: "50%", background: "#22C55E" }} />
              <span style={{ fontFamily: "Outfit, sans-serif", fontSize: 12, color: "#22C55E", fontWeight: 500 }}>Online · Always available</span>
            </div>
          </div>
          <div style={{ marginLeft: "auto", display: "flex", gap: 8 }}>
            <button onClick={() => setMessages([messages[0]])} style={{ background: C.ivory, border: "none", borderRadius: 8, padding: "7px 14px", cursor: "pointer", fontFamily: "Outfit, sans-serif", fontSize: 12, color: C.muted, fontWeight: 600 }}>Clear Chat</button>
          </div>
        </div>
        {/* Messages */}
        <div style={{ flex: 1, overflow: "auto", padding: "28px 28px 16px" }}>
          {messages.map((msg, i) => <ChatBubble key={i} msg={msg} />)}
          {typing && (
            <div style={{ display: "flex", justifyContent: "flex-start", marginBottom: 16 }}>
              <div style={{ width: 34, height: 34, borderRadius: 10, flexShrink: 0, marginRight: 10, background: `linear-gradient(135deg, ${C.blue}, #5B7FFF)`, display: "flex", alignItems: "center", justifyContent: "center", color: "white", fontFamily: "DM Serif Display, serif", fontSize: 16 }}>B</div>
              <div style={{ background: C.white, borderRadius: "4px 18px 18px 18px", padding: "12px 16px", boxShadow: "0 2px 12px rgba(0,0,0,0.06)", border: `1px solid ${C.borderLight}` }}><TypingDots /></div>
            </div>
          )}
          <div ref={chatEnd} />
        </div>
        {/* Quick chips */}
        <div style={{ padding: "8px 28px", display: "flex", gap: 8, overflowX: "auto" }}>
          {QUICK_CHIPS.slice(0, 4).map(c => (
            <button key={c} onClick={() => send(c)} style={{
              background: C.white, border: `1.5px solid ${C.border}`, borderRadius: 20, padding: "6px 14px", cursor: "pointer",
              fontFamily: "Outfit, sans-serif", fontSize: 12, color: C.muted, fontWeight: 500,
              whiteSpace: "nowrap", transition: "all 0.15s", flexShrink: 0,
            }}
              onMouseEnter={e => { e.currentTarget.style.borderColor = C.blue; e.currentTarget.style.color = C.blue; e.currentTarget.style.background = C.blueLight; }}
              onMouseLeave={e => { e.currentTarget.style.borderColor = C.border; e.currentTarget.style.color = C.muted; e.currentTarget.style.background = C.white; }}>
              {c}
            </button>
          ))}
        </div>
        {/* Input */}
        <div style={{ padding: "12px 28px 20px", background: C.white, borderTop: `1px solid ${C.border}`, display: "flex", gap: 10, alignItems: "flex-end" }}>
          <button onClick={startVoice} style={{
            width: 44, height: 44, borderRadius: 11, background: listening ? C.blue : C.ivory,
            border: `1.5px solid ${listening ? C.blue : C.border}`, cursor: "pointer", flexShrink: 0,
            display: "flex", alignItems: "center", justifyContent: "center", fontSize: 18, transition: "all 0.2s",
          }}>🎙️</button>
          <textarea
            value={input}
            onChange={e => setInput(e.target.value)}
            onKeyDown={e => { if (e.key === "Enter" && !e.shiftKey) { e.preventDefault(); send(input); } }}
            placeholder="Ask Buddy anything about MIT Vishwaprayag University..."
            style={{
              flex: 1, background: C.ivory, border: `1.5px solid ${C.border}`, borderRadius: 12,
              padding: "11px 16px", fontFamily: "Outfit, sans-serif", fontSize: 14, color: C.charcoal,
              resize: "none", outline: "none", height: 44, lineHeight: "22px",
              transition: "border-color 0.2s",
            }}
            onFocus={e => e.target.style.borderColor = C.blue}
            onBlur={e => e.target.style.borderColor = C.border}
          />
          <button onClick={() => send(input)} disabled={!input.trim() || typing} style={{
            width: 44, height: 44, borderRadius: 11, background: input.trim() && !typing ? C.blue : C.border,
            border: "none", cursor: input.trim() && !typing ? "pointer" : "default", flexShrink: 0,
            display: "flex", alignItems: "center", justifyContent: "center",
            color: "white", fontSize: 18, transition: "all 0.2s",
            boxShadow: input.trim() && !typing ? `0 4px 14px ${C.blue}40` : "none",
          }}>→</button>
        </div>
      </div>
    </div>
  );

  // ─── DASHBOARD ────────────────────────────────────────────────────
  const Dashboard = () => {
    const stats = [
      { label: "Programs Offered", value: "4", sub: "UG & PG", color: C.blue, icon: "📚" },
      { label: "Scholarships", value: "3", sub: "Up to 100% waiver", color: C.gold, icon: "🏆" },
      { label: "Min. Attendance", value: "75%", sub: "Required for all", color: "#059669", icon: "📅" },
      { label: "Admission Steps", value: "12", sub: "Simple process", color: "#7C3AED", icon: "📋" },
    ];
    const recentQ = [
      "What programs are offered?", "Is scholarship available for B.Tech?",
      "How to apply for MCA?", "What is the minimum attendance?", "Is hostel available?",
    ];
    const admSteps = [
      "Visit Portal", "Fill Form", "Pay ₹1000", "Get Login ID", "Complete App",
      "Interview", "Entrance Exam", "Check Result", "Offer Letter", "Pay Installment", "Doc Verify", "Get PRN",
    ];
    return (
      <div style={{ maxWidth: 1200, margin: "0 auto", padding: "40px 40px" }}>
        <div style={{ marginBottom: 36 }}>
          <h1 style={{ fontFamily: "DM Serif Display, serif", fontSize: 36, color: C.charcoal, marginBottom: 6 }}>Dashboard</h1>
          <p style={{ fontFamily: "Outfit, sans-serif", fontSize: 15, color: C.muted }}>School of Computing · MIT Vishwaprayag University</p>
        </div>
        {/* Stats */}
        <div style={{ display: "grid", gridTemplateColumns: "repeat(4, 1fr)", gap: 20, marginBottom: 32 }}>
          {stats.map(({ label, value, sub, color, icon }) => (
            <div key={label} style={{ background: C.white, borderRadius: 16, padding: "24px 22px", border: `1px solid ${C.borderLight}`, boxShadow: "0 2px 16px rgba(0,0,0,0.04)" }}>
              <div style={{ display: "flex", justifyContent: "space-between", alignItems: "flex-start", marginBottom: 14 }}>
                <div style={{ width: 44, height: 44, borderRadius: 12, background: color + "15", display: "flex", alignItems: "center", justifyContent: "center", fontSize: 22 }}>{icon}</div>
              </div>
              <div style={{ fontFamily: "DM Serif Display, serif", fontSize: 36, color: color, lineHeight: 1 }}>{value}</div>
              <div style={{ fontFamily: "Outfit, sans-serif", fontWeight: 600, fontSize: 14, color: C.charcoal, marginTop: 4 }}>{label}</div>
              <div style={{ fontFamily: "Outfit, sans-serif", fontSize: 12, color: C.muted, marginTop: 2 }}>{sub}</div>
            </div>
          ))}
        </div>
        <div style={{ display: "grid", gridTemplateColumns: "1fr 1fr", gap: 24, marginBottom: 24 }}>
          {/* Recent Questions */}
          <div style={{ background: C.white, borderRadius: 16, padding: "24px", border: `1px solid ${C.borderLight}` }}>
            <h3 style={{ fontFamily: "DM Serif Display, serif", fontSize: 22, color: C.charcoal, marginBottom: 18 }}>Popular Topics</h3>
            {recentQ.map((q, i) => (
              <div key={i} onClick={() => { setPage("chat"); setTimeout(() => send(q), 400); }} style={{
                display: "flex", alignItems: "center", gap: 12, padding: "10px 12px", borderRadius: 10,
                cursor: "pointer", transition: "all 0.15s", marginBottom: 4,
              }}
                onMouseEnter={e => e.currentTarget.style.background = C.blueLight}
                onMouseLeave={e => e.currentTarget.style.background = "none"}>
                <div style={{ width: 28, height: 28, borderRadius: 8, background: C.blue + "15", display: "flex", alignItems: "center", justifyContent: "center", fontFamily: "Outfit, sans-serif", fontSize: 12, fontWeight: 700, color: C.blue, flexShrink: 0 }}>{i + 1}</div>
                <span style={{ fontFamily: "Outfit, sans-serif", fontSize: 13.5, color: C.charcoal, fontWeight: 500 }}>{q}</span>
                <span style={{ marginLeft: "auto", color: C.blue, fontSize: 14 }}>→</span>
              </div>
            ))}
          </div>
          {/* Programs Overview */}
          <div style={{ background: C.white, borderRadius: 16, padding: "24px", border: `1px solid ${C.borderLight}` }}>
            <h3 style={{ fontFamily: "DM Serif Display, serif", fontSize: 22, color: C.charcoal, marginBottom: 18 }}>Programs at a Glance</h3>
            {PROGRAMS_DATA.map(({ name, fee, duration, color }) => (
              <div key={name} style={{ display: "flex", alignItems: "center", gap: 12, padding: "10px 0", borderBottom: `1px solid ${C.borderLight}` }}>
                <div style={{ width: 10, height: 10, borderRadius: "50%", background: color, flexShrink: 0 }} />
                <span style={{ fontFamily: "Outfit, sans-serif", fontWeight: 600, fontSize: 13.5, color: C.charcoal, flex: 1 }}>{name}</span>
                <span style={{ fontFamily: "Outfit, sans-serif", fontSize: 12, color: C.muted }}>{duration}</span>
                <span style={{ fontFamily: "Outfit, sans-serif", fontWeight: 700, fontSize: 13, color }}>{fee}/yr</span>
              </div>
            ))}
          </div>
        </div>
        {/* Admission Timeline */}
        <div style={{ background: C.white, borderRadius: 16, padding: "28px", border: `1px solid ${C.borderLight}` }}>
          <h3 style={{ fontFamily: "DM Serif Display, serif", fontSize: 22, color: C.charcoal, marginBottom: 22 }}>Admission Process — 12 Steps</h3>
          <div style={{ display: "flex", flexWrap: "wrap", gap: 12 }}>
            {admSteps.map((step, i) => (
              <div key={step} style={{ display: "flex", alignItems: "center", gap: 8, background: C.blueLight, borderRadius: 20, padding: "7px 14px" }}>
                <div style={{ width: 22, height: 22, borderRadius: "50%", background: C.blue, color: "white", display: "flex", alignItems: "center", justifyContent: "center", fontFamily: "Outfit, sans-serif", fontSize: 11, fontWeight: 700, flexShrink: 0 }}>{i + 1}</div>
                <span style={{ fontFamily: "Outfit, sans-serif", fontSize: 12.5, fontWeight: 600, color: C.blue }}>{step}</span>
              </div>
            ))}
          </div>
        </div>
        {/* Quick Action */}
        <div style={{ marginTop: 24, background: `linear-gradient(135deg, ${C.blue}, #4F75FF)`, borderRadius: 16, padding: "28px 32px", display: "flex", alignItems: "center", justifyContent: "space-between" }}>
          <div>
            <h3 style={{ fontFamily: "DM Serif Display, serif", fontSize: 26, color: "white", marginBottom: 6 }}>Need more info?</h3>
            <p style={{ fontFamily: "Outfit, sans-serif", fontSize: 14, color: "rgba(255,255,255,0.75)" }}>Chat with Buddy for instant answers on any university topic.</p>
          </div>
          <button onClick={() => setPage("chat")} style={{
            background: "white", color: C.blue, border: "none", borderRadius: 11, padding: "13px 28px",
            fontFamily: "Outfit, sans-serif", fontWeight: 700, fontSize: 15,
            cursor: "pointer", boxShadow: "0 4px 16px rgba(0,0,0,0.15)", flexShrink: 0,
          }}>Open Chat →</button>
        </div>
      </div>
    );
  };

  return (
    <div style={{ fontFamily: "Outfit, sans-serif", background: C.bg, minHeight: "100vh" }}>
      <Nav />
      {page === "home" && (
        <>
          <Hero />
          <QuickAccess />
          <ChatPreview />
          <WhyBuddy />
          <ProgramsSection />
          <FAQSection />
          <Footer />
        </>
      )}
      {page === "chat" && <ChatPage />}
      {page === "dashboard" && <Dashboard />}
    </div>
  );
}
