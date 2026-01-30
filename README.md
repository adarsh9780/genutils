<svg viewBox="0 0 800 500" xmlns="http://www.w3.org/2000/svg">
  <defs>
    <marker id="arrowhead" markerWidth="10" markerHeight="7" refX="0" refY="3.5" orient="auto">
      <polygon points="0 0, 10 3.5, 0 7" fill="#4A5568" />
    </marker>
  </defs>

  <rect width="800" height="500" fill="#F7FAFC" rx="10"/>

  <g transform="translate(50, 220)">
    <rect width="120" height="60" rx="8" fill="#E2E8F0" stroke="#4A5568" stroke-width="1.5" />
    <text x="60" y="25" text-anchor="middle" font-family="sans-serif" font-weight="bold" font-size="14" fill="#2D3748">User Input</text>
    <text x="60" y="45" text-anchor="middle" font-family="sans-serif" font-size="10" fill="#718096">ex: gem diamonds</text>
    <line x1="120" y1="30" x2="170" y2="30" stroke="#4A5568" stroke-width="2" marker-end="url(#arrowhead)" />
  </g>

  <g transform="translate(200, 40)">
    <path d="M0,10 A60,15 0 0,1 120,10 L120,70 A60,15 0 0,1 0,70 Z" fill="#EBF8FF" stroke="#3182CE" stroke-width="2" />
    <ellipse cx="60" cy="10" rx="60" ry="15" fill="#BEE3F8" stroke="#3182CE" stroke-width="2" />
    <text x="60" y="45" text-anchor="middle" font-family="sans-serif" font-weight="bold" font-size="14" fill="#2C5282">Memory</text>
    <line x1="45" y1="85" x2="45" y2="165" stroke="#3182CE" stroke-width="1.5" marker-end="url(#arrowhead)" />
    <line x1="75" y1="165" x2="75" y2="85" stroke="#3182CE" stroke-width="1.5" marker-end="url(#arrowhead)" />
  </g>

  <g transform="translate(180, 210)">
    <rect width="160" height="80" rx="12" fill="#FFF5F5" stroke="#E53E3E" stroke-width="2" />
    <text x="80" y="45" text-anchor="middle" font-family="sans-serif" font-weight="bold" font-size="16" fill="#9B2C2C">LLM (Brain)</text>
    <text x="80" y="65" text-anchor="middle" font-family="sans-serif" font-size="12" font-style="italic" fill="#C53030">Reasoning Engine</text>
  </g>

  <g transform="translate(450, 40)">
    <rect width="200" height="130" rx="10" fill="#F0FFF4" stroke="#38A169" stroke-width="2" />
    <text x="100" y="25" text-anchor="middle" font-family="sans-serif" font-weight="bold" font-size="14" fill="#22543D">Tools</text>
    <line x1="20" y1="35" x2="180" y2="35" stroke="#38A169" stroke-width="1" />
    <text x="30" y="60" font-family="monospace" font-size="12" fill="#276749">• SQLRetriever</text>
    <text x="30" y="80" font-family="monospace" font-size="12" fill="#276749">• EntityDisambig</text>
    <text x="30" y="100" font-family="monospace" font-size="12" fill="#276749">• ThemeEngine</text>
    <text x="30" y="120" font-family="monospace" font-size="12" fill="#276749">• TrendDetection</text>
    <path d="M-10,140 Q-50,110 -110,170" fill="none" stroke="#38A169" stroke-width="1.5" stroke-dasharray="4" marker-end="url(#arrowhead)" transform="translate(100,0)"/>
    <path d="M-120,190 Q-60,130 0,160" fill="none" stroke="#38A169" stroke-width="1.5" stroke-dasharray="4" marker-end="url(#arrowhead)" transform="translate(100,0)"/>
  </g>

  <g transform="translate(450, 220)">
    <rect width="140" height="60" rx="8" fill="#FAF5FF" stroke="#805AD5" stroke-width="1.5" />
    <text x="70" y="35" text-anchor="middle" font-family="sans-serif" font-weight="bold" font-size="14" fill="#44337A">Summary</text>
    <line x1="-110" y1="60" x2="-20" y2="40" stroke="#805AD5" stroke-width="1.5" stroke-dasharray="4" marker-end="url(#arrowhead)" transform="translate(0,0)"/>
  </g>

  <g transform="translate(640, 220)">
    <rect width="130" height="60" rx="8" fill="#EDF2F7" stroke="#2D3748" stroke-width="1.5" />
    <text x="65" y="35" text-anchor="middle" font-family="sans-serif" font-weight="bold" font-size="14" fill="#1A202C">Chat Agent</text>
    <line x1="-50" y1="30" x2="-10" y2="30" stroke="#2D3748" stroke-width="1.5" stroke-dasharray="4" marker-end="url(#arrowhead)" />
    <line x1="130" y1="30" x2="160" y2="30" stroke="#2D3748" stroke-width="2" marker-end="url(#arrowhead)" />
  </g>
</svg>
