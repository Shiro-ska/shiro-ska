import React, { useState, useMemo } from 'react';

// Tailored initial data for "shiro-ska"
const initialData = {
  template: 'creative', // 'creative', 'professional', 'minimalist', 'techlead'
  name: 'Shiro',
  githubUser: 'shiro-ska',
  title: 'Full-Stack Developer & Tech Enthusiast',
  subtitle: 'Crafting clean code, interactive web apps, and modern digital experiences.',
  location: 'Surakarta, Indonesia',
  bio: 'Hello! I am a passionate developer based in Surakarta. I love building open-source projects, learning new web frameworks, and exploring system architecture.',
  currentWork: 'Building awesome web apps & open-source tools',
  learning: 'Next.js 14, Rust & Cloud Architecture',
  askMeAbout: 'React, TypeScript, Node.js, and Web Development',
  pronouns: 'he/him',
  
  // GitHub Stats Options
  showStats: true,
  showLanguages: true,
  showStreak: true,
  showVisitors: true,
  showQuote: true,
  showSnake: true,
  statsTheme: 'tokyonight', // tokyonight, radical, dark, gruvbox, dracula, nord, synthwave
  statsLayout: 'compact',

  // Social Links
  socials: {
    github: 'shiro-ska',
    linkedin: 'shiro-ska',
    twitter: 'shiro_ska',
    instagram: 'shiro_ska',
    youtube: '@shiro-ska',
    discord: 'shiro_ska#0001',
    email: 'shiro@example.com',
    portfolio: 'https://github.com/shiro-ska',
    buymeacoffee: 'shiroska',
    kofi: 'shiroska'
  },

  // Tech Stack Badges Selected
  selectedTech: [
    'javascript', 'typescript', 'react', 'nextjs', 'tailwindcss', 
    'nodejs', 'express', 'python', 'postgresql', 'mongodb', 
    'docker', 'git', 'figma'
  ]
};

// Categorized Tech Database with Shields.io badges
const TECH_DATABASE = [
  // Languages
  { id: 'javascript', name: 'JavaScript', category: 'Languages', badge: 'JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black' },
  { id: 'typescript', name: 'TypeScript', category: 'Languages', badge: 'TypeScript-007ACC?style=for-the-badge&logo=typescript&logoColor=white' },
  { id: 'python', name: 'Python', category: 'Languages', badge: 'Python-3776AB?style=for-the-badge&logo=python&logoColor=white' },
  { id: 'php', name: 'PHP', category: 'Languages', badge: 'PHP-777BB4?style=for-the-badge&logo=php&logoColor=white' },
  { id: 'cpp', name: 'C++', category: 'Languages', badge: 'C%2B%2B-00599C?style=for-the-badge&logo=cplusplus&logoColor=white' },
  { id: 'go', name: 'Go', category: 'Languages', badge: 'Go-00ADD8?style=for-the-badge&logo=go&logoColor=white' },
  { id: 'rust', name: 'Rust', category: 'Languages', badge: 'Rust-000000?style=for-the-badge&logo=rust&logoColor=white' },
  { id: 'html5', name: 'HTML5', category: 'Languages', badge: 'HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white' },
  { id: 'css3', name: 'CSS3', category: 'Languages', badge: 'CSS3-1572B6?style=for-the-badge&logo=css3&logoColor=white' },

  // Frontend
  { id: 'react', name: 'React', category: 'Frontend', badge: 'React-20232A?style=for-the-badge&logo=react&logoColor=61DAFB' },
  { id: 'nextjs', name: 'Next.js', category: 'Frontend', badge: 'Next-000000?style=for-the-badge&logo=nextdotjs&logoColor=white' },
  { id: 'vuejs', name: 'Vue.js', category: 'Frontend', badge: 'Vue.js-35495E?style=for-the-badge&logo=vuedotjs&logoColor=4FC08D' },
  { id: 'tailwindcss', name: 'Tailwind CSS', category: 'Frontend', badge: 'Tailwind_CSS-38B2AC?style=for-the-badge&logo=tailwind-css&logoColor=white' },
  { id: 'bootstrap', name: 'Bootstrap', category: 'Frontend', badge: 'Bootstrap-563D7C?style=for-the-badge&logo=bootstrap&logoColor=white' },
  { id: 'vite', name: 'Vite', category: 'Frontend', badge: 'Vite-646CFF?style=for-the-badge&logo=vite&logoColor=white' },
  { id: 'astro', name: 'Astro', category: 'Frontend', badge: 'Astro-BC52EE?style=for-the-badge&logo=astro&logoColor=white' },

  // Backend
  { id: 'nodejs', name: 'Node.js', category: 'Backend', badge: 'Node.js-43853D?style=for-the-badge&logo=node.js&logoColor=white' },
  { id: 'express', name: 'Express.js', category: 'Backend', badge: 'Express.js-000000?style=for-the-badge&logo=express&logoColor=white' },
  { id: 'laravel', name: 'Laravel', category: 'Backend', badge: 'Laravel-FF2D20?style=for-the-badge&logo=laravel&logoColor=white' },
  { id: 'nestjs', name: 'NestJS', category: 'Backend', badge: 'NestJS-E0234E?style=for-the-badge&logo=nestjs&logoColor=white' },
  { id: 'fastapi', name: 'FastAPI', category: 'Backend', badge: 'FastAPI-009688?style=for-the-badge&logo=fastapi&logoColor=white' },

  // Database
  { id: 'postgresql', name: 'PostgreSQL', category: 'Database', badge: 'PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white' },
  { id: 'mongodb', name: 'MongoDB', category: 'Database', badge: 'MongoDB-4EA94B?style=for-the-badge&logo=mongodb&logoColor=white' },
  { id: 'mysql', name: 'MySQL', category: 'Database', badge: 'MySQL-00000F?style=for-the-badge&logo=mysql&logoColor=white' },
  { id: 'redis', name: 'Redis', category: 'Database', badge: 'Redis-DC382D?style=for-the-badge&logo=redis&logoColor=white' },
  { id: 'supabase', name: 'Supabase', category: 'Database', badge: 'Supabase-3ECF8E?style=for-the-badge&logo=supabase&logoColor=white' },

  // Cloud & DevOps
  { id: 'docker', name: 'Docker', category: 'Cloud & DevOps', badge: 'Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white' },
  { id: 'aws', name: 'AWS', category: 'Cloud & DevOps', badge: 'Amazon_AWS-232F3E?style=for-the-badge&logo=amazon-aws&logoColor=white' },
  { id: 'vercel', name: 'Vercel', category: 'Cloud & DevOps', badge: 'Vercel-000000?style=for-the-badge&logo=vercel&logoColor=white' },
  { id: 'linux', name: 'Linux', category: 'Cloud & DevOps', badge: 'Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black' },

  // Tools & Design
  { id: 'git', name: 'Git', category: 'Tools', badge: 'Git-F05032?style=for-the-badge&logo=git&logoColor=white' },
  { id: 'github', name: 'GitHub', category: 'Tools', badge: 'GitHub-100000?style=for-the-badge&logo=github&logoColor=white' },
  { id: 'figma', name: 'Figma', category: 'Tools', badge: 'Figma-F24E1E?style=for-the-badge&logo=figma&logoColor=white' },
  { id: 'postman', name: 'Postman', category: 'Tools', badge: 'Postman-FF6C37?style=for-the-badge&logo=postman&logoColor=white' }
];

export default function App() {
  const [data, setData] = useState(initialData);
  const [activeTab, setActiveTab] = useState('bio'); // 'bio', 'tech', 'stats', 'socials'
  const [activeView, setActiveView] = useState('editor'); // 'editor', 'tutorial'
  const [previewTab, setPreviewTab] = useState('visual'); // 'visual', 'code'
  const [searchTech, setSearchTech] = useState('');
  const [selectedCategory, setSelectedCategory] = useState('All');
  const [toast, setToast] = useState(null);

  const showToast = (msg) => {
    setToast(msg);
    setTimeout(() => setToast(null), 3000);
  };

  const handleChange = (field, value) => {
    setData((prev) => ({ ...prev, [field]: value }));
  };

  const handleSocialChange = (key, value) => {
    setData((prev) => ({
      ...prev,
      socials: { ...prev.socials, [key]: value }
    }));
  };

  const toggleTech = (techId) => {
    setData((prev) => {
      const exists = prev.selectedTech.includes(techId);
      const updated = exists
        ? prev.selectedTech.filter((id) => id !== techId)
        : [...prev.selectedTech, techId];
      return { ...prev, selectedTech: updated };
    });
  };

  const categories = ['All', 'Languages', 'Frontend', 'Backend', 'Database', 'Cloud & DevOps', 'Tools'];

  const filteredTech = useMemo(() => {
    return TECH_DATABASE.filter((item) => {
      const matchesCategory = selectedCategory === 'All' || item.category === selectedCategory;
      const matchesSearch = item.name.toLowerCase().includes(searchTech.toLowerCase());
      return matchesCategory && matchesSearch;
    });
  }, [searchTech, selectedCategory]);

  // Markdown Generator
  const markdownCode = useMemo(() => {
    const {
      template, name, title, subtitle, location, bio,
      currentWork, learning, askMeAbout, pronouns,
      githubUser, showStats, showLanguages, showStreak, showVisitors,
      showQuote, showSnake, statsTheme, statsLayout, socials, selectedTech
    } = data;

    let md = '';

    const techBadgesMd = selectedTech
      .map((id) => {
        const item = TECH_DATABASE.find((t) => t.id === id);
        return item ? `![${item.name}](https://img.shields.io/badge/${item.badge})` : '';
      })
      .filter(Boolean)
      .join(' ');

    const socialBadges = [];
    if (socials.linkedin) socialBadges.push(`[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://linkedin.com/in/${socials.linkedin})`);
    if (socials.twitter) socialBadges.push(`[![Twitter](https://img.shields.io/badge/Twitter-1DA1F2?style=for-the-badge&logo=twitter&logoColor=white)](https://twitter.com/${socials.twitter})`);
    if (socials.instagram) socialBadges.push(`[![Instagram](https://img.shields.io/badge/Instagram-E4405F?style=for-the-badge&logo=instagram&logoColor=white)](https://instagram.com/${socials.instagram})`);
    if (socials.youtube) socialBadges.push(`[![YouTube](https://img.shields.io/badge/YouTube-FF0000?style=for-the-badge&logo=youtube&logoColor=white)](https://youtube.com/${socials.youtube})`);
    if (socials.discord) socialBadges.push(`[![Discord](https://img.shields.io/badge/Discord-7289DA?style=for-the-badge&logo=discord&logoColor=white)](https://discord.com)`);
    if (socials.portfolio) socialBadges.push(`[![Portfolio](https://img.shields.io/badge/Portfolio-2563EB?style=for-the-badge&logo=google-chrome&logoColor=white)](${socials.portfolio})`);
    if (socials.email) socialBadges.push(`[![Email](https://img.shields.io/badge/Email-D14836?style=for-the-badge&logo=gmail&logoColor=white)](mailto:${socials.email})`);
    if (socials.buymeacoffee) socialBadges.push(`[![BuyMeACoffee](https://img.shields.io/badge/Buy%20Me%20A%20Coffee-FFDD00?style=for-the-badge&logo=buy-me-a-coffee&logoColor=black)](https://buymeacoffee.com/${socials.buymeacoffee})`);
    if (socials.kofi) socialBadges.push(`[![Ko-Fi](https://img.shields.io/badge/Ko--fi-F16436?style=for-the-badge&logo=ko-fi&logoColor=white)](https://ko-fi.com/${socials.kofi})`);

    const socialMd = socialBadges.join(' ');

    if (template === 'creative') {
      md += `<h1 align="center">Hi 👋, I'm ${name}</h1>\n`;
      if (title) md += `<h3 align="center">${title}</h3>\n`;
      if (subtitle) md += `<p align="center"><em>${subtitle}</em></p>\n\n`;

      if (showVisitors && githubUser) {
        md += `<p align="center">\n  <img src="https://komarev.com/ghpvc/?username=${githubUser}&label=Profile%20Views&color=0e75b6&style=flat" alt="Profile Views" />\n</p>\n\n`;
      }

      if (socialMd) {
        md += `<p align="center">\n${socialMd}\n</p>\n\n`;
      }

      md += `<img src="https://user-images.githubusercontent.com/73097560/115834477-dbab4500-a447-11eb-908a-139a6edaec5c.gif" width="100%" />\n\n`;

      md += `## 🚀 About Me\n\n`;
      if (bio) md += `${bio}\n\n`;
      if (currentWork) md += `- 🔭 I’m currently working on **${currentWork}**\n`;
      if (learning) md += `- 🌱 I’m currently learning **${learning}**\n`;
      if (askMeAbout) md += `- 💬 Ask me about **${askMeAbout}**\n`;
      if (location) md += `- 📍 Based in **${location}**\n`;
      if (pronouns) md += `- ⚡ Pronouns: **${pronouns}**\n`;
      md += `\n`;

      if (techBadgesMd) {
        md += `## 💻 Tech Stack & Skills\n\n<p align="left">\n${techBadgesMd}\n</p>\n\n`;
      }

      if (githubUser && (showStats || showLanguages || showStreak || showSnake)) {
        md += `## 📊 GitHub Analytics\n\n`;
        md += `<p align="center">\n`;
        if (showStats) md += `  <img src="https://github-readme-stats.vercel.app/api?username=${githubUser}&show_icons=true&theme=${statsTheme}" alt="Stats" />\n`;
        if (showLanguages) md += `  <img src="https://github-readme-stats.vercel.app/api/top-langs/?username=${githubUser}&layout=${statsLayout}&theme=${statsTheme}" alt="Languages" />\n`;
        if (showStreak) md += `  <img src="https://github-readme-streak-stats.herokuapp.com/?user=${githubUser}&theme=${statsTheme}" alt="Streak" />\n`;
        md += `</p>\n\n`;

        if (showSnake) {
          md += `### 🐍 Contribution Snake Game\n\n`;
          md += `![Snake animation](https://raw.githubusercontent.com/${githubUser}/${githubUser}/output/github-contribution-grid-snake.svg)\n\n`;
        }
      }

      if (showQuote) {
        md += `## 💬 Daily Dev Quote\n\n`;
        md += `![Dev Quote](https://quotes-github-readme.vercel.app/api?type=horizontal&theme=${statsTheme})\n\n`;
      }
    } else {
      // Standard Professional Template fallback
      md += `# ${name}\n\n`;
      if (title) md += `### ${title}\n\n`;
      if (bio) md += `${bio}\n\n`;
      if (location) md += `📍 Based in ${location}\n\n`;
      if (techBadgesMd) md += `### Tech Stack\n${techBadgesMd}\n\n`;
      if (socialMd) md += `### Connect\n${socialMd}\n\n`;
      if (githubUser && showStats) {
        md += `### GitHub Stats\n![Stats](https://github-readme-stats.vercel.app/api?username=${githubUser}&show_icons=true&theme=${statsTheme})\n`;
      }
    }

    return md.trim();
  }, [data]);

  // Copy Markdown
  const copyToClipboard = (text, message = 'Copied to Clipboard!') => {
    const textArea = document.createElement('textarea');
    textArea.value = text;
    textArea.style.position = 'fixed';
    textArea.style.left = '-9999px';
    document.body.appendChild(textArea);
    textArea.focus();
    textArea.select();

    try {
      document.execCommand('copy');
      showToast(message);
    } catch (err) {
      showToast('Failed to copy');
    }
    document.body.removeChild(textArea);
  };

  // Download README.md
  const downloadReadme = () => {
    const element = document.createElement('a');
    const file = new Blob([markdownCode], { type: 'text/markdown' });
    element.href = URL.createObjectURL(file);
    element.download = 'README.md';
    document.body.appendChild(element);
    element.click();
    document.body.removeChild(element);
    showToast('Downloaded README.md');
  };

  const snakeWorkflowYaml = `name: Generate Snake Contribution Animation

on:
  schedule:
    - cron: "0 0 * * *" # Runs daily at midnight
  workflow_dispatch:

jobs:
  generate:
    permissions:
      contents: write
    runs-on: ubuntu-latest
    timeout-minutes: 5

    steps:
      - name: generate github-contribution-grid-snake.svg
        uses: Platane/snk/svg-only@v3
        with:
          github_user_name: ${data.githubUser || 'shiro-ska'}
          outputs: |
            dist/github-contribution-grid-snake.svg
            dist/github-contribution-grid-snake-dark.svg?palette=github-dark

      - name: push github-contribution-grid-snake.svg to the output branch
        uses: crazy-max/ghaction-github-pages@v3.1.0
        with:
          target_branch: output
          build_dir: dist
        env:
          GITHUB_TOKEN: \${{ secrets.GITHUB_TOKEN }}`;

  return (
    <div className="min-h-screen bg-slate-950 text-slate-100 flex flex-col font-sans">
      
      {/* Toast Alert */}
      {toast && (
        <div className="fixed top-5 right-5 z-50 bg-emerald-600 text-white px-4 py-2.5 rounded-lg shadow-xl font-medium text-xs transition-all animate-bounce flex items-center gap-2">
          <span>✨</span> {toast}
        </div>
      )}

      {/* Navigation Header */}
      <header className="border-b border-slate-800 bg-slate-900/90 backdrop-blur sticky top-0 z-40 px-4 sm:px-6 py-3 flex flex-wrap items-center justify-between gap-4">
        <div className="flex items-center gap-3">
          <div className="bg-gradient-to-tr from-indigo-500 to-purple-600 p-2 rounded-xl text-white shadow-lg shadow-indigo-500/20">
            <svg className="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path strokeLinecap="round" strokeLinejoin="round" strokeWidth="2" d="M10 20l4-16m4 4l4 4-4 4M6 16l-4-4 4-4" />
            </svg>
          </div>
          <div>
            <h1 className="font-bold text-base text-white flex items-center gap-2">
              GitHub Profile README Builder
              <span className="text-[10px] bg-indigo-950 text-indigo-300 border border-indigo-700/60 px-2 py-0.5 rounded-full font-mono">
                @{data.githubUser}
              </span>
            </h1>
            <p className="text-[11px] text-slate-400">Tailored profile generator for github.com/shiro-ska</p>
          </div>
        </div>

        {/* View Mode Switches & Actions */}
        <div className="flex items-center gap-2">
          <button
            onClick={() => setActiveView('editor')}
            className={`px-3 py-1.5 text-xs font-semibold rounded-lg transition ${
              activeView === 'editor'
                ? 'bg-slate-800 text-white border border-slate-700'
                : 'text-slate-400 hover:text-white'
            }`}
          >
            📝 Form Editor
          </button>
          <button
            onClick={() => setActiveView('tutorial')}
            className={`px-3 py-1.5 text-xs font-semibold rounded-lg transition flex items-center gap-1.5 ${
              activeView === 'tutorial'
                ? 'bg-purple-600 text-white shadow-lg shadow-purple-600/30'
                : 'bg-slate-800 text-purple-300 hover:bg-slate-700'
            }`}
          >
            🚀 How to Setup (shiro-ska)
          </button>

          <div className="h-4 w-[1px] bg-slate-800 my-auto mx-1" />

          <button
            onClick={() => copyToClipboard(markdownCode, 'Markdown copied!')}
            className="px-3.5 py-1.5 text-xs font-semibold bg-indigo-600 hover:bg-indigo-500 text-white rounded-lg shadow-lg shadow-indigo-600/20 transition flex items-center gap-1.5"
          >
            <svg className="w-3.5 h-3.5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path strokeLinecap="round" strokeLinejoin="round" strokeWidth="2" d="M8 5H6a2 2 0 00-2 2v12a2 2 0 002 2h10a2 2 0 002-2v-1M8 5a2 2 0 002 2h2a2 2 0 002-2M8 5a2 2 0 012-2h2a2 2 0 012 2m0 0h2a2 2 0 012 2v3m2 4H10m0 0l3-3m-3 3l3 3" />
            </svg>
            Copy Markdown
          </button>
          <button
            onClick={downloadReadme}
            className="px-3.5 py-1.5 text-xs font-semibold bg-emerald-600 hover:bg-emerald-500 text-white rounded-lg shadow-lg shadow-emerald-600/20 transition flex items-center gap-1.5"
          >
            Download
          </button>
        </div>
      </header>

      {/* Main Workspace Layout */}
      <div className="flex-1 grid grid-cols-1 lg:grid-cols-12 overflow-hidden">
        
        {/* LEFT PANEL: Editor or Tutorial */}
        <div className="lg:col-span-5 border-r border-slate-800 bg-slate-900/50 flex flex-col h-full overflow-hidden">
          
          {activeView === 'editor' ? (
            <>
              {/* Tabs for Editor Form Sections */}
              <div className="flex border-b border-slate-800 bg-slate-900 px-2 pt-2 gap-1 overflow-x-auto no-scrollbar">
                {[
                  { id: 'bio', label: 'Profile Info', icon: '👤' },
                  { id: 'tech', label: 'Tech Stack', icon: '⚡' },
                  { id: 'stats', label: 'Widgets & Theme', icon: '📊' },
                  { id: 'socials', label: 'Social Links', icon: '🔗' }
                ].map((tab) => (
                  <button
                    key={tab.id}
                    onClick={() => setActiveTab(tab.id)}
                    className={`px-3 py-2 text-xs font-semibold rounded-t-lg transition whitespace-nowrap flex items-center gap-1.5 border-t border-x ${
                      activeTab === tab.id
                        ? 'bg-slate-950 text-indigo-400 border-indigo-500/50 border-b-transparent'
                        : 'text-slate-400 hover:text-slate-200 border-transparent bg-transparent'
                    }`}
                  >
                    <span>{tab.icon}</span>
                    {tab.label}
                  </button>
                ))}
              </div>

              {/* Form Input Area */}
              <div className="flex-1 overflow-y-auto p-4 sm:p-5 space-y-5">
                
                {activeTab === 'bio' && (
                  <div className="space-y-4">
                    <div>
                      <label className="block text-xs font-semibold text-slate-300 mb-1">Display Name</label>
                      <input
                        type="text"
                        value={data.name}
                        onChange={(e) => handleChange('name', e.target.value)}
                        className="w-full bg-slate-950 border border-slate-800 rounded-lg px-3 py-2 text-xs text-slate-200 focus:outline-none focus:border-indigo-500"
                      />
                    </div>

                    <div>
                      <label className="block text-xs font-semibold text-slate-300 mb-1">Headline / Role</label>
                      <input
                        type="text"
                        value={data.title}
                        onChange={(e) => handleChange('title', e.target.value)}
                        className="w-full bg-slate-950 border border-slate-800 rounded-lg px-3 py-2 text-xs text-slate-200 focus:outline-none focus:border-indigo-500"
                      />
                    </div>

                    <div>
                      <label className="block text-xs font-semibold text-slate-300 mb-1">Location</label>
                      <input
                        type="text"
                        value={data.location}
                        onChange={(e) => handleChange('location', e.target.value)}
                        className="w-full bg-slate-950 border border-slate-800 rounded-lg px-3 py-2 text-xs text-slate-200 focus:outline-none focus:border-indigo-500"
                      />
                    </div>

                    <div>
                      <label className="block text-xs font-semibold text-slate-300 mb-1">Bio Summary</label>
                      <textarea
                        rows="3"
                        value={data.bio}
                        onChange={(e) => handleChange('bio', e.target.value)}
                        className="w-full bg-slate-950 border border-slate-800 rounded-lg px-3 py-2 text-xs text-slate-200 focus:outline-none focus:border-indigo-500"
                      />
                    </div>

                    <div className="space-y-3 pt-2">
                      <h4 className="text-xs font-bold text-indigo-400 uppercase">Activity Bullets</h4>
                      
                      <div>
                        <label className="block text-xs text-slate-400 mb-1">🔭 Currently Working On</label>
                        <input
                          type="text"
                          value={data.currentWork}
                          onChange={(e) => handleChange('currentWork', e.target.value)}
                          className="w-full bg-slate-950 border border-slate-800 rounded-lg px-3 py-2 text-xs text-slate-200 focus:outline-none focus:border-indigo-500"
                        />
                      </div>

                      <div>
                        <label className="block text-xs text-slate-400 mb-1">🌱 Currently Learning</label>
                        <input
                          type="text"
                          value={data.learning}
                          onChange={(e) => handleChange('learning', e.target.value)}
                          className="w-full bg-slate-950 border border-slate-800 rounded-lg px-3 py-2 text-xs text-slate-200 focus:outline-none focus:border-indigo-500"
                        />
                      </div>

                      <div>
                        <label className="block text-xs text-slate-400 mb-1">💬 Ask Me About</label>
                        <input
                          type="text"
                          value={data.askMeAbout}
                          onChange={(e) => handleChange('askMeAbout', e.target.value)}
                          className="w-full bg-slate-950 border border-slate-800 rounded-lg px-3 py-2 text-xs text-slate-200 focus:outline-none focus:border-indigo-500"
                        />
                      </div>
                    </div>
                  </div>
                )}

                {activeTab === 'tech' && (
                  <div className="space-y-4">
                    <div>
                      <input
                        type="text"
                        value={searchTech}
                        onChange={(e) => setSearchTech(e.target.value)}
                        placeholder="Search frameworks, languages, tools..."
                        className="w-full bg-slate-950 border border-slate-800 rounded-lg px-3 py-2 text-xs text-slate-200 focus:outline-none focus:border-indigo-500 mb-2"
                      />

                      <div className="flex gap-1 overflow-x-auto pb-2 no-scrollbar">
                        {categories.map((cat) => (
                          <button
                            key={cat}
                            onClick={() => setSelectedCategory(cat)}
                            className={`px-2.5 py-1 text-[11px] rounded-md transition whitespace-nowrap ${
                              selectedCategory === cat
                                ? 'bg-indigo-600 text-white font-semibold'
                                : 'bg-slate-800 text-slate-400 hover:text-white'
                            }`}
                          >
                            {cat}
                          </button>
                        ))}
                      </div>
                    </div>

                    <div className="grid grid-cols-2 gap-2 max-h-[380px] overflow-y-auto pr-1">
                      {filteredTech.map((tech) => {
                        const isSelected = data.selectedTech.includes(tech.id);
                        return (
                          <button
                            key={tech.id}
                            onClick={() => toggleTech(tech.id)}
                            className={`p-2.5 rounded-lg text-left text-xs flex items-center justify-between border transition ${
                              isSelected
                                ? 'bg-indigo-950/60 border-indigo-500 text-indigo-200 font-medium'
                                : 'bg-slate-800/40 border-slate-800 text-slate-300 hover:bg-slate-800'
                            }`}
                          >
                            <span className="truncate">{tech.name}</span>
                            <span className={`w-4 h-4 rounded-full flex items-center justify-center text-[10px] ${
                              isSelected ? 'bg-indigo-500 text-white' : 'border border-slate-600'
                            }`}>
                              {isSelected ? '✓' : ''}
                            </span>
                          </button>
                        );
                      })}
                    </div>
                  </div>
                )}

                {activeTab === 'stats' && (
                  <div className="space-y-4">
                    <div>
                      <label className="block text-xs font-semibold text-slate-300 mb-1">GitHub Username</label>
                      <input
                        type="text"
                        value={data.githubUser}
                        onChange={(e) => handleChange('githubUser', e.target.value)}
                        className="w-full bg-slate-950 border border-slate-800 rounded-lg px-3 py-2 text-xs text-slate-200 focus:outline-none focus:border-indigo-500 font-mono"
                      />
                    </div>

                    <div>
                      <label className="block text-xs font-semibold text-slate-300 mb-1">Stats Card Theme</label>
                      <select
                        value={data.statsTheme}
                        onChange={(e) => handleChange('statsTheme', e.target.value)}
                        className="w-full bg-slate-950 border border-slate-800 rounded-lg px-3 py-2 text-xs text-slate-200 focus:outline-none focus:border-indigo-500"
                      >
                        {['tokyonight', 'radical', 'dark', 'gruvbox', 'dracula', 'nord', 'synthwave', 'merko', 'onedark'].map((t) => (
                          <option key={t} value={t}>{t}</option>
                        ))}
                      </select>
                    </div>

                    <div className="space-y-2 pt-2">
                      <h4 className="text-xs font-bold text-indigo-400 uppercase">Widgets Toggle</h4>
                      {[
                        { id: 'showStats', label: 'GitHub Overall Stats Card' },
                        { id: 'showLanguages', label: 'Top Languages Card' },
                        { id: 'showStreak', label: 'Commit Streak Counter' },
                        { id: 'showVisitors', label: 'Profile Visitor Counter' },
                        { id: 'showSnake', label: 'Snake Contribution Animation' },
                        { id: 'showQuote', label: 'Daily Dev Quote Banner' }
                      ].map((w) => (
                        <label key={w.id} className="flex items-center justify-between p-2.5 rounded-lg bg-slate-800/40 border border-slate-800 cursor-pointer text-xs">
                          <span className="text-slate-300">{w.label}</span>
                          <input
                            type="checkbox"
                            checked={data[w.id]}
                            onChange={(e) => handleChange(w.id, e.target.checked)}
                            className="w-4 h-4 rounded accent-indigo-500"
                          />
                        </label>
                      ))}
                    </div>
                  </div>
                )}

                {activeTab === 'socials' && (
                  <div className="grid grid-cols-1 gap-3">
                    {[
                      { key: 'linkedin', label: 'LinkedIn Username' },
                      { key: 'twitter', label: 'Twitter / X Handle' },
                      { key: 'instagram', label: 'Instagram Handle' },
                      { key: 'youtube', label: 'YouTube Handle' },
                      { key: 'discord', label: 'Discord User' },
                      { key: 'email', label: 'Email Address' },
                      { key: 'portfolio', label: 'Portfolio Website' },
                      { key: 'buymeacoffee', label: 'BuyMeACoffee Handle' },
                      { key: 'kofi', label: 'Ko-Fi Username' }
                    ].map((s) => (
                      <div key={s.key}>
                        <label className="block text-[11px] text-slate-400 mb-1">{s.label}</label>
                        <input
                          type="text"
                          value={data.socials[s.key] || ''}
                          onChange={(e) => handleSocialChange(s.key, e.target.value)}
                          className="w-full bg-slate-950 border border-slate-800 rounded-lg px-3 py-1.5 text-xs text-slate-200 focus:outline-none focus:border-indigo-500"
                        />
                      </div>
                    ))}
                  </div>
                )}

              </div>
            </>
          ) : (
            /* STEP BY STEP TUTORIAL FOR shiro-ska */
            <div className="flex-1 overflow-y-auto p-5 space-y-6">
              <div>
                <span className="text-[10px] bg-purple-950 text-purple-300 border border-purple-800 px-2 py-0.5 rounded-full font-semibold">
                  Guide for @{data.githubUser || 'shiro-ska'}
                </span>
                <h2 className="text-lg font-bold text-white mt-1">Langkah Memasang Profile README</h2>
                <p className="text-xs text-slate-400 mt-1 leading-relaxed">
                  Ikuti panduan berikut untuk mengaktifkan halaman depan GitHub kamu agar terlihat rapi dan profesional.
                </p>
              </div>

              {/* Step 1 */}
              <div className="bg-slate-950 p-4 rounded-xl border border-slate-800 space-y-2">
                <div className="flex items-center gap-2 text-xs font-bold text-indigo-400">
                  <span className="bg-indigo-600 text-white w-5 h-5 rounded-full flex items-center justify-center text-[10px]">1</span>
                  Buat Special Repository
                </div>
                <p className="text-xs text-slate-300 leading-relaxed">
                  Buka GitHub dan buat repository baru bernama persis sesuai username kamu:
                </p>
                <div className="bg-slate-900 p-2.5 rounded-lg border border-slate-800 font-mono text-xs text-emerald-400 flex items-center justify-between">
                  <span>github.com/new ➔ Repo Name: <strong>{data.githubUser || 'shiro-ska'}</strong></span>
                </div>
                <p className="text-[11px] text-slate-400">
                  ⚠️ Pastikan repository diset ke <strong>Public</strong> dan centang opsi <strong>Add a README file</strong>.
                </p>
              </div>

              {/* Step 2 */}
              <div className="bg-slate-950 p-4 rounded-xl border border-slate-800 space-y-2">
                <div className="flex items-center gap-2 text-xs font-bold text-indigo-400">
                  <span className="bg-indigo-600 text-white w-5 h-5 rounded-full flex items-center justify-center text-[10px]">2</span>
                  Salin & Commit File README.md
                </div>
                <p className="text-xs text-slate-300 leading-relaxed">
                  Klik tombol <strong>"Copy Markdown"</strong> di pojok kanan atas aplikasi ini, lalu edit file <code className="text-indigo-300">README.md</code> pada repository <code className="text-indigo-300">{data.githubUser}/{data.githubUser}</code> di GitHub, kemudian Commit changes.
                </p>
              </div>

              {/* Step 3 - Snake Animation GitHub Action */}
              <div className="bg-slate-950 p-4 rounded-xl border border-slate-800 space-y-3">
                <div className="flex items-center justify-between">
                  <div className="flex items-center gap-2 text-xs font-bold text-indigo-400">
                    <span className="bg-indigo-600 text-white w-5 h-5 rounded-full flex items-center justify-center text-[10px]">3</span>
                    Setup GitHub Action Animasi Ular (Snake)
                  </div>
                  <button
                    onClick={() => copyToClipboard(snakeWorkflowYaml, 'YAML Workflow Copied!')}
                    className="text-[11px] text-indigo-400 hover:underline font-semibold"
                  >
                    Copy YAML
                  </button>
                </div>
                <p className="text-xs text-slate-300 leading-relaxed">
                  Agar animasi game ular berjalan otomatis, buat file workflow baru di repo kamu pada lokasi:
                  <br />
                  <code className="text-indigo-300 font-mono text-[11px]">.github/workflows/snake.yml</code>
                </p>

                <div className="bg-slate-900 p-3 rounded-lg border border-slate-800 overflow-x-auto">
                  <pre className="font-mono text-[10px] text-slate-300 leading-tight">
                    {snakeWorkflowYaml}
                  </pre>
                </div>

                <p className="text-[11px] text-slate-400">
                  💡 Setelah menyimpan file YAML, buka tab <strong>Actions</strong> di GitHub repo kamu, pilih <strong>Generate Snake Contribution Animation</strong>, lalu klik <strong>Run workflow</strong>.
                </p>
              </div>

            </div>
          )}

        </div>

        {/* RIGHT PANEL: Live Visual Preview / Code View */}
        <div className="lg:col-span-7 flex flex-col h-full bg-slate-950 overflow-hidden">
          
          <div className="border-b border-slate-800 bg-slate-900/80 px-4 py-2 flex items-center justify-between">
            <div className="flex items-center gap-1.5">
              <button
                onClick={() => setPreviewTab('visual')}
                className={`px-3 py-1 text-xs font-semibold rounded-lg transition ${
                  previewTab === 'visual' ? 'bg-indigo-600 text-white' : 'text-slate-400 bg-slate-800'
                }`}
              >
                👁️ Visual Preview
              </button>
              <button
                onClick={() => setPreviewTab('code')}
                className={`px-3 py-1 text-xs font-semibold rounded-lg transition ${
                  previewTab === 'code' ? 'bg-indigo-600 text-white' : 'text-slate-400 bg-slate-800'
                }`}
              >
                💻 Markdown Code
              </button>
            </div>
            <span className="text-[11px] text-slate-400 font-mono">
              github.com/{data.githubUser || 'shiro-ska'}
            </span>
          </div>

          <div className="flex-1 overflow-y-auto p-4 sm:p-6">
            {previewTab === 'visual' ? (
              <div className="max-w-2xl mx-auto bg-slate-900 border border-slate-800 rounded-2xl p-6 shadow-2xl text-slate-200 space-y-5">
                
                {/* Visual Preview Header */}
                <div className="text-center space-y-1">
                  <h1 className="text-2xl font-extrabold text-white">Hi 👋, I'm {data.name}</h1>
                  {data.title && <p className="text-sm font-semibold text-indigo-400">{data.title}</p>}
                  {data.subtitle && <p className="text-xs text-slate-400 italic">{data.subtitle}</p>}
                  
                  {data.showVisitors && (
                    <div className="flex justify-center pt-2">
                      <img
                        src={`https://komarev.com/ghpvc/?username=${data.githubUser || 'shiro-ska'}&label=Profile%20Views&color=0e75b6&style=flat`}
                        alt="Views"
                        className="h-5"
                      />
                    </div>
                  )}
                </div>

                {/* Banner Divider GIF */}
                <img
                  src="https://user-images.githubusercontent.com/73097560/115834477-dbab4500-a447-11eb-908a-139a6edaec5c.gif"
                  alt="Divider"
                  className="w-full rounded"
                />

                {/* About Section */}
                <div className="space-y-2">
                  <h3 className="text-xs font-bold text-slate-300 uppercase tracking-wider">🚀 About Me</h3>
                  {data.bio && <p className="text-xs text-slate-300 leading-relaxed bg-slate-950/60 p-3 rounded-lg border border-slate-800">{data.bio}</p>}
                  
                  <ul className="text-xs text-slate-300 space-y-1 pt-1">
                    {data.currentWork && <li>🔭 Currently working on: <strong className="text-white">{data.currentWork}</strong></li>}
                    {data.learning && <li>🌱 Currently learning: <strong className="text-white">{data.learning}</strong></li>}
                    {data.askMeAbout && <li>💬 Ask me about: <strong className="text-white">{data.askMeAbout}</strong></li>}
                    {data.location && <li>📍 Based in: <strong className="text-white">{data.location}</strong></li>}
                  </ul>
                </div>

                {/* Tech Stack */}
                {data.selectedTech.length > 0 && (
                  <div className="space-y-2 pt-2">
                    <h3 className="text-xs font-bold text-slate-300 uppercase tracking-wider">💻 Tech Stack & Skills</h3>
                    <div className="flex flex-wrap gap-1.5">
                      {data.selectedTech.map((id) => {
                        const item = TECH_DATABASE.find((t) => t.id === id);
                        return item ? (
                          <img
                            key={id}
                            src={`https://img.shields.io/badge/${item.badge}`}
                            alt={item.name}
                            className="h-6 rounded"
                          />
                        ) : null;
                      })}
                    </div>
                  </div>
                )}

                {/* GitHub Analytics */}
                <div className="space-y-3 pt-2">
                  <h3 className="text-xs font-bold text-slate-300 uppercase tracking-wider">📊 GitHub Analytics</h3>
                  <div className="flex flex-wrap gap-2 justify-center bg-slate-950/50 p-3 rounded-xl border border-slate-800">
                    {data.showStats && (
                      <img
                        src={`https://github-readme-stats.vercel.app/api?username=${data.githubUser || 'shiro-ska'}&show_icons=true&theme=${data.statsTheme}`}
                        alt="Stats"
                        className="max-w-full rounded"
                      />
                    )}
                    {data.showLanguages && (
                      <img
                        src={`https://github-readme-stats.vercel.app/api/top-langs/?username=${data.githubUser || 'shiro-ska'}&layout=compact&theme=${data.statsTheme}`}
                        alt="Languages"
                        className="max-w-full rounded"
                      />
                    )}
                    {data.showStreak && (
                      <img
                        src={`https://github-readme-streak-stats.herokuapp.com/?user=${data.githubUser || 'shiro-ska'}&theme=${data.statsTheme}`}
                        alt="Streak"
                        className="max-w-full rounded"
                      />
                    )}
                  </div>
                </div>

              </div>
            ) : (
              <textarea
                readOnly
                value={markdownCode}
                className="w-full h-full min-h-[500px] bg-slate-950 font-mono text-xs text-slate-200 p-4 rounded-xl border border-slate-800 focus:outline-none resize-none leading-relaxed"
              />
            )}
          </div>

        </div>

      </div>
    </div>
  );
}
