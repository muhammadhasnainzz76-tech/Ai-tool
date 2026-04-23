import { useState, useEffect, useRef } from "react";

const FONTS = `@import url('https://fonts.googleapis.com/css2?family=Syne:wght@400;600;700;800&family=Outfit:wght@300;400;500;600;700&display=swap');`;

const AI_TOOLS = [
  // Content & Writing
  { id: 1, name: "AI Story Writer", desc: "Generate compelling stories in seconds", cat: "Writing", icon: "✍️", badge: "HOT", uses: "2.1M", color: "#FF6B6B" },
  { id: 2, name: "AI Blog Post Generator", desc: "Full SEO blogs in one click", cat: "Writing", icon: "📝", badge: "NEW", uses: "980K", color: "#FF8E53" },
  { id: 3, name: "AI Email Writer", desc: "Professional emails instantly", cat: "Writing", icon: "📧", uses: "1.4M", color: "#FFC107" },
  { id: 4, name: "AI Text Summarizer", desc: "Summarize any text in seconds", cat: "Writing", icon: "📋", uses: "3.2M", color: "#4CAF50" },
  { id: 5, name: "AI Poem Generator", desc: "Beautiful poems on any topic", cat: "Writing", icon: "🎭", uses: "670K", color: "#9C27B0" },
  { id: 6, name: "AI Essay Writer", desc: "Academic essays with citations", cat: "Writing", icon: "🎓", badge: "HOT", uses: "1.8M", color: "#2196F3" },
  { id: 7, name: "AI Script Writer", desc: "YouTube & movie scripts", cat: "Writing", icon: "🎬", uses: "540K", color: "#E91E63" },
  { id: 8, name: "AI Paraphraser", desc: "Rewrite any text uniquely", cat: "Writing", icon: "🔄", uses: "4.1M", color: "#00BCD4" },
  // Social & Marketing
  { id: 9, name: "AI Instagram Caption", desc: "Viral captions with hashtags", cat: "Social", icon: "📸", badge: "VIRAL", uses: "5.2M", color: "#E91E63" },
  { id: 10, name: "AI YouTube Title Generator", desc: "Clickbait titles that rank", cat: "Social", icon: "▶️", badge: "HOT", uses: "3.8M", color: "#FF0000" },
  { id: 11, name: "AI Hashtag Generator", desc: "Top trending hashtags", cat: "Social", icon: "#️⃣", uses: "2.9M", color: "#1DA1F2" },
  { id: 12, name: "AI TikTok Script", desc: "Viral TikTok video scripts", cat: "Social", icon: "🎵", badge: "VIRAL", uses: "4.4M", color: "#010101" },
  { id: 13, name: "AI Ad Copy Generator", desc: "High-converting ad copies", cat: "Social", icon: "📢", uses: "1.1M", color: "#FF6B35" },
  { id: 14, name: "AI Product Description", desc: "Amazon & Shopify descriptions", cat: "Social", icon: "🛍️", uses: "2.3M", color: "#FF9900" },
  { id: 15, name: "AI Twitter/X Thread", desc: "Viral Twitter threads fast", cat: "Social", icon: "🐦", badge: "NEW", uses: "1.6M", color: "#1DA1F2" },
  { id: 16, name: "AI LinkedIn Post", desc: "Professional LinkedIn content", cat: "Social", icon: "💼", uses: "890K", color: "#0077B5" },
  // Image & Design
  { id: 17, name: "AI Image Generator", desc: "Text to stunning images", cat: "Image", icon: "🖼️", badge: "HOT", uses: "8.7M", color: "#FF6B6B" },
  { id: 18, name: "AI Logo Maker", desc: "Professional logos in seconds", cat: "Image", icon: "✨", badge: "HOT", uses: "3.4M", color: "#9C27B0" },
  { id: 19, name: "AI Background Remover", desc: "Remove BG in one click", cat: "Image", icon: "🎨", uses: "6.2M", color: "#4CAF50" },
  { id: 20, name: "AI Thumbnail Maker", desc: "YouTube thumbnails that get clicks", cat: "Image", icon: "🖥️", badge: "VIRAL", uses: "2.8M", color: "#FF5722" },
  { id: 21, name: "AI Photo Enhancer", desc: "Upscale & enhance any photo", cat: "Image", icon: "📷", uses: "4.1M", color: "#00BCD4" },
  { id: 22, name: "AI Avatar Generator", desc: "AI profile pictures", cat: "Image", icon: "👤", badge: "NEW", uses: "3.6M", color: "#673AB7" },
  { id: 23, name: "AI Color Palette", desc: "Beautiful color combos", cat: "Image", icon: "🎨", uses: "1.2M", color: "#FF4081" },
  { id: 24, name: "AI Meme Generator", desc: "Viral memes in seconds", cat: "Image", icon: "😂", badge: "VIRAL", uses: "7.1M", color: "#FFC107" },
  // SEO & Business
  { id: 25, name: "AI SEO Keyword Tool", desc: "Find high-traffic keywords", cat: "SEO", icon: "🔍", badge: "HOT", uses: "2.6M", color: "#4CAF50" },
  { id: 26, name: "AI Meta Description", desc: "SEO meta tags generator", cat: "SEO", icon: "🏷️", uses: "1.8M", color: "#2196F3" },
  { id: 27, name: "AI Content Calendar", desc: "Plan 30-day content easily", cat: "SEO", icon: "📅", uses: "760K", color: "#FF9800" },
  { id: 28, name: "AI Business Name Generator", desc: "Unique business names fast", cat: "SEO", icon: "🏢", uses: "2.1M", color: "#E91E63" },
  { id: 29, name: "AI SWOT Analysis", desc: "Instant business analysis", cat: "SEO", icon: "📊", uses: "430K", color: "#607D8B" },
  { id: 30, name: "AI Press Release", desc: "Professional PR in minutes", cat: "SEO", icon: "📰", uses: "340K", color: "#795548" },
  // Career & Education
  { id: 31, name: "AI Resume Builder", desc: "ATS-friendly resumes", cat: "Career", icon: "📄", badge: "HOT", uses: "4.9M", color: "#2196F3" },
  { id: 32, name: "AI Cover Letter", desc: "Perfect cover letters", cat: "Career", icon: "💌", uses: "2.7M", color: "#9C27B0" },
  { id: 33, name: "AI Interview Q&A", desc: "Prepare for any interview", cat: "Career", icon: "🎯", badge: "NEW", uses: "1.3M", color: "#FF5722" },
  { id: 34, name: "AI Quiz Generator", desc: "Educational quizzes instantly", cat: "Career", icon: "❓", uses: "890K", color: "#4CAF50" },
  { id: 35, name: "AI Lesson Planner", desc: "Teacher lesson plans fast", cat: "Career", icon: "📚", uses: "560K", color: "#FFC107" },
  // Voice & Audio
  { id: 36, name: "AI Voice Generator", desc: "Text to natural speech", cat: "Audio", icon: "🎙️", badge: "HOT", uses: "3.1M", color: "#E91E63" },
  { id: 37, name: "AI Music Generator", desc: "Create music from text", cat: "Audio", icon: "🎵", badge: "NEW", uses: "2.4M", color: "#9C27B0" },
  { id: 38, name: "AI Podcast Script", desc: "Full podcast scripts", cat: "Audio", icon: "🎧", uses: "670K", color: "#FF9800" },
  { id: 39, name: "AI Sound Effects", desc: "Custom audio effects", cat: "Audio", icon: "🔊", uses: "420K", color: "#00BCD4" },
  // Code & Tech
  { id: 40, name: "AI Code Generator", desc: "Generate code in any language", cat: "Code", icon: "💻", badge: "HOT", uses: "5.8M", color: "#4CAF50" },
  { id: 41, name: "AI Code Debugger", desc: "Fix bugs automatically", cat: "Code", icon: "🐛", uses: "3.2M", color: "#FF5722" },
  { id: 42, name: "AI SQL Query Builder", desc: "Complex SQL queries fast", cat: "Code", icon: "🗄️", uses: "1.4M", color: "#2196F3" },
  { id: 43, name: "AI Website Builder", desc: "Full websites from text", cat: "Code", icon: "🌐", badge: "NEW", uses: "2.1M", color: "#9C27B0" },
  { id: 44, name: "AI API Documentation", desc: "Auto-generate API docs", cat: "Code", icon: "📡", uses: "780K", color: "#607D8B" },
  // Fun & Viral
  { id: 45, name: "AI Roast Generator", desc: "Hilarious roasts for friends", cat: "Fun", icon: "🔥", badge: "VIRAL", uses: "9.2M", color: "#FF6B35" },
  { id: 46, name: "AI Joke Generator", desc: "Fresh jokes on demand", cat: "Fun", icon: "😄", uses: "4.6M", color: "#FFC107" },
  { id: 47, name: "AI Horoscope Generator", desc: "AI-powered daily horoscope", cat: "Fun", icon: "⭐", badge: "VIRAL", uses: "6.8M", color: "#9C27B0" },
  { id: 48, name: "AI Name Meaning", desc: "Deep meaning of any name", cat: "Fun", icon: "🌟", uses: "3.4M", color: "#E91E63" },
  { id: 49, name: "AI Dream Interpreter", desc: "What does your dream mean?", cat: "Fun", icon: "💭", badge: "NEW", uses: "2.9M", color: "#673AB7" },
  { id: 50, name: "AI Chatbot Assistant", desc: "Your 24/7 AI companion", cat: "Fun", icon: "🤖", badge: "HOT", uses: "11.2M", color: "#2196F3" },
];

const CATEGORIES = ["All", "Writing", "Social", "Image", "SEO", "Career", "Audio", "Code", "Fun"];

const TRENDING = [
  { rank: 1, name: "AI Chatbot Assistant", uses: "11.2M", change: "+234%" },
  { rank: 2, name: "AI Roast Generator", uses: "9.2M", change: "+189%" },
  { rank: 3, name: "AI Image Generator", uses: "8.7M", change: "+156%" },
  { rank: 4, name: "AI Horoscope Generator", uses: "6.8M", change: "+143%" },
  { rank: 5, name: "AI Background Remover", uses: "6.2M", change: "+121%" },
];

const BLOG_POSTS = [
  { title: "Best Free AI Tools for Students in 2025", reads: "124K", time: "8 min", tag: "Education", img: "🎓" },
  { title: "Top 10 AI Image Generators with No Signup", reads: "98K", time: "6 min", tag: "Images", img: "🖼️" },
  { title: "How AI Tools Are Changing Blogging Forever", reads: "87K", time: "7 min", tag: "Writing", img: "✍️" },
  { title: "AI Tools for Pakistani Freelancers: Complete Guide", reads: "76K", time: "10 min", tag: "Freelance", img: "🇵🇰" },
  { title: "Free AI Logo Makers vs Paid: Which Wins?", reads: "65K", time: "5 min", tag: "Design", img: "✨" },
  { title: "Viral TikTok Ideas Using AI in 2025", reads: "153K", time: "4 min", tag: "Social", img: "🎵" },
];

function useCountUp(target, duration = 2000, start = false) {
  const [count, setCount] = useState(0);
  useEffect(() => {
    if (!start) return;
    let startTime = null;
    const step = (timestamp) => {
      if (!startTime) startTime = timestamp;
      const progress = Math.min((timestamp - startTime) / duration, 1);
      setCount(Math.floor(progress * target));
      if (progress < 1) requestAnimationFrame(step);
    };
    requestAnimationFrame(step);
  }, [target, duration, start]);
  return count;
}

function AdBlock({ type = "banner", label = "Advertisement" }) {
  const styles = {
    banner: { width: "100%", height: "90px", fontSize: "13px" },
    rectangle: { width: "100%", height: "250px", fontSize: "13px" },
    sidebar: { width: "100%", height: "600px", fontSize: "13px" },
  };
  return (
    <div style={{
      background: "rgba(255,255,255,0.03)",
      border: "1px dashed rgba(255,255,255,0.1)",
      borderRadius: "12px",
      display: "flex",
      flexDirection: "column",
      alignItems: "center",
      justifyContent: "center",
      color: "rgba(255,255,255,0.2)",
      fontFamily: "'Outfit', sans-serif",
      gap: "6px",
      ...styles[type]
    }}>
      <span style={{ fontSize: "10px", letterSpacing: "3px", textTransform: "uppercase" }}>{label}</span>
      <span style={{ fontSize: "11px" }}>Google AdSense · High RPM Zone</span>
    </div>
  );
}

function ToolCard({ tool, onClick }) {
  const [hovered, setHovered] = useState(false);
  return (
    <div
      onClick={() => onClick(tool)}
      onMouseEnter={() => setHovered(true)}
      onMouseLeave={() => setHovered(false)}
      style={{
        background: hovered
          ? `linear-gradient(135deg, rgba(255,255,255,0.08) 0%, rgba(255,255,255,0.03) 100%)`
          : "rgba(255,255,255,0.03)",
        border: hovered ? `1px solid ${tool.color}55` : "1px solid rgba(255,255,255,0.07)",
        borderRadius: "16px",
        padding: "20px",
        cursor: "pointer",
        transition: "all 0.25s cubic-bezier(0.4,0,0.2,1)",
        transform: hovered ? "translateY(-4px)" : "translateY(0)",
        boxShadow: hovered ? `0 12px 40px ${tool.color}22` : "none",
        position: "relative",
        overflow: "hidden",
      }}
    >
      {tool.badge && (
        <span style={{
          position: "absolute", top: "12px", right: "12px",
          background: tool.badge === "VIRAL" ? "#FF6B35" : tool.badge === "HOT" ? "#FF4444" : "#4CAF50",
          color: "#fff", fontSize: "9px", fontWeight: "700",
          padding: "2px 7px", borderRadius: "20px", letterSpacing: "1px",
          fontFamily: "'Outfit', sans-serif"
        }}>{tool.badge}</span>
      )}
      <div style={{
        width: "44px", height: "44px", borderRadius: "12px",
        background: `${tool.color}22`, border: `1px solid ${tool.color}44`,
        display: "flex", alignItems: "center", justifyContent: "center",
        fontSize: "20px", marginBottom: "12px"
      }}>{tool.icon}</div>
      <div style={{ fontFamily: "'Syne', sans-serif", fontWeight: "700", fontSize: "14px", color: "#fff", marginBottom: "6px" }}>{tool.name}</div>
      <div style={{ fontFamily: "'Outfit', sans-serif", fontSize: "12px", color: "rgba(255,255,255,0.45)", lineHeight: "1.4", marginBottom: "12px" }}>{tool.desc}</div>
      <div style={{ display: "flex", alignItems: "center", gap: "4px" }}>
        <div style={{ width: "6px", height: "6px", borderRadius: "50%", background: tool.color, boxShadow: `0 0 6px ${tool.color}` }} />
        <span style={{ fontFamily: "'Outfit', sans-serif", fontSize: "11px", color: "rgba(255,255,255,0.35)" }}>{tool.uses} uses</span>
      </div>
      <div style={{
        position: "absolute", bottom: 0, left: 0, right: 0, height: "2px",
        background: `linear-gradient(90deg, transparent, ${tool.color}, transparent)`,
        opacity: hovered ? 1 : 0, transition: "opacity 0.3s"
      }} />
    </div>
  );
}

function ToolModal({ tool, onClose }) {
  const [input, setInput] = useState("");
  const [output, setOutput] = useState("");
  const [loading, setLoading] = useState(false);

  const runTool = async () => {
    if (!input.trim()) return;
    setLoading(true);
    setOutput("");
    try {
      const res = await fetch("https://api.anthropic.com/v1/messages", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({
          model: "claude-sonnet-4-20250514",
          max_tokens: 1000,
          messages: [{
            role: "user",
            content: `You are an expert ${tool.name} AI tool. The user says: "${input}". Produce a high-quality, detailed, ready-to-use output. Be creative and professional. Format your response clearly.`
          }]
        })
      });
      const data = await res.json();
      setOutput(data.content?.[0]?.text || "No output generated.");
    } catch {
      setOutput("⚠️ Error connecting to AI. Please try again.");
    }
    setLoading(false);
  };

  return (
    <div style={{
      position: "fixed", inset: 0, background: "rgba(0,0,0,0.85)", backdropFilter: "blur(16px)",
      zIndex: 9999, display: "flex", alignItems: "center", justifyContent: "center", padding: "20px"
    }} onClick={onClose}>
      <div onClick={e => e.stopPropagation()} style={{
        background: "linear-gradient(135deg, #0D0F1A 0%, #111827 100%)",
        border: `1px solid ${tool.color}44`,
        borderRadius: "24px", padding: "32px", width: "100%", maxWidth: "680px",
        maxHeight: "90vh", overflow: "auto",
        boxShadow: `0 0 80px ${tool.color}22, 0 32px 64px rgba(0,0,0,0.8)`
      }}>
        <div style={{ display: "flex", justifyContent: "space-between", alignItems: "flex-start", marginBottom: "24px" }}>
          <div style={{ display: "flex", gap: "16px", alignItems: "center" }}>
            <div style={{
              width: "56px", height: "56px", borderRadius: "16px",
              background: `${tool.color}22`, border: `1px solid ${tool.color}55`,
              display: "flex", alignItems: "center", justifyContent: "center", fontSize: "26px"
            }}>{tool.icon}</div>
            <div>
              <div style={{ fontFamily: "'Syne', sans-serif", fontWeight: "800", fontSize: "20px", color: "#fff" }}>{tool.name}</div>
              <div style={{ fontFamily: "'Outfit', sans-serif", fontSize: "13px", color: "rgba(255,255,255,0.4)" }}>{tool.desc}</div>
            </div>
          </div>
          <button onClick={onClose} style={{
            background: "rgba(255,255,255,0.07)", border: "1px solid rgba(255,255,255,0.1)",
            color: "#fff", width: "36px", height: "36px", borderRadius: "50%",
            cursor: "pointer", fontSize: "16px", display: "flex", alignItems: "center", justifyContent: "center"
          }}>✕</button>
        </div>

        <AdBlock type="banner" label="Advertisement — Google AdSense" />
        <div style={{ height: "16px" }} />

        <textarea
          value={input}
          onChange={e => setInput(e.target.value)}
          placeholder={`Enter your prompt for ${tool.name}...`}
          rows={4}
          style={{
            width: "100%", background: "rgba(255,255,255,0.05)", border: "1px solid rgba(255,255,255,0.1)",
            borderRadius: "12px", padding: "16px", color: "#fff", fontSize: "14px",
            fontFamily: "'Outfit', sans-serif", resize: "vertical", outline: "none",
            boxSizing: "border-box", marginBottom: "12px",
            transition: "border-color 0.2s"
          }}
          onFocus={e => e.target.style.borderColor = tool.color}
          onBlur={e => e.target.style.borderColor = "rgba(255,255,255,0.1)"}
        />
        <button onClick={runTool} disabled={loading} style={{
          width: "100%", padding: "14px", borderRadius: "12px",
          background: loading ? "rgba(255,255,255,0.05)" : `linear-gradient(135deg, ${tool.color}, ${tool.color}bb)`,
          border: "none", color: "#fff", fontFamily: "'Syne', sans-serif",
          fontWeight: "700", fontSize: "15px", cursor: loading ? "not-allowed" : "pointer",
          transition: "all 0.2s", display: "flex", alignItems: "center", justifyContent: "center", gap: "8px"
        }}>
          {loading ? (
            <><span style={{ animation: "spin 1s linear infinite", display: "inline-block" }}>⚙️</span> Generating...</>
          ) : `✨ Generate with ${tool.name}`}
        </button>

        {output && (
          <div style={{
            marginTop: "20px", background: "rgba(255,255,255,0.04)", border: "1px solid rgba(255,255,255,0.08)",
            borderRadius: "12px", padding: "20px"
          }}>
            <div style={{ fontFamily: "'Syne', sans-serif", fontWeight: "700", fontSize: "12px", color: "rgba(255,255,255,0.4)", letterSpacing: "2px", marginBottom: "12px" }}>AI OUTPUT</div>
            <div style={{ fontFamily: "'Outfit', sans-serif", fontSize: "14px", color: "rgba(255,255,255,0.85)", lineHeight: "1.8", whiteSpace: "pre-wrap" }}>{output}</div>
            <button onClick={() => navigator.clipboard?.writeText(output)} style={{
              marginTop: "16px", background: "rgba(255,255,255,0.07)", border: "1px solid rgba(255,255,255,0.1)",
              color: "rgba(255,255,255,0.7)", padding: "8px 16px", borderRadius: "8px",
              cursor: "pointer", fontFamily: "'Outfit', sans-serif", fontSize: "12px"
            }}>📋 Copy Output</button>
          </div>
        )}
      </div>
    </div>
  );
}

export default function AIToolsHubPro() {
  const [darkMode] = useState(true);
  const [search, setSearch] = useState("");
  const [activecat, setActivecat] = useState("All");
  const [selectedTool, setSelectedTool] = useState(null);
  const [newsletter, setNewsletter] = useState(false);
  const [email, setEmail] = useState("");
  const [subscribed, setSubscribed] = useState(false);
  const [countersStarted, setCountersStarted] = useState(false);
  const heroRef = useRef();
  const [mobileMenuOpen, setMobileMenuOpen] = useState(false);

  const tools = useCountUp(250, 1500, countersStarted);
  const users = useCountUp(4800000, 2000, countersStarted);
  const monthly = useCountUp(12000000, 2200, countersStarted);
  const countries = useCountUp(180, 1800, countersStarted);

  useEffect(() => {
    const timer = setTimeout(() => setNewsletter(true), 8000);
    return () => clearTimeout(timer);
  }, []);

  useEffect(() => {
    const obs = new IntersectionObserver(([e]) => { if (e.isIntersecting) setCountersStarted(true); }, { threshold: 0.3 });
    if (heroRef.current) obs.observe(heroRef.current);
    return () => obs.disconnect();
  }, []);

  const filtered = AI_TOOLS.filter(t => {
    const catMatch = activecat === "All" || t.cat === activecat;
    const searchMatch = !search || t.name.toLowerCase().includes(search.toLowerCase()) || t.desc.toLowerCase().includes(search.toLowerCase()) || t.cat.toLowerCase().includes(search.toLowerCase());
    return catMatch && searchMatch;
  });

  const formatNum = (n) => {
    if (n >= 1000000) return (n / 1000000).toFixed(1) + "M";
    if (n >= 1000) return (n / 1000).toFixed(0) + "K";
    return n.toString();
  };

  return (
    <div style={{
      background: "#060810",
      minHeight: "100vh",
      color: "#fff",
      fontFamily: "'Outfit# Ai-tool
All In one ai tools 
