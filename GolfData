import streamlit as st
import pandas as pd
import plotly.express as px
import plotly.graph_objects as go

# =========================
# PAGE CONFIG
# =========================
st.set_page_config(
    page_title="Golf Analytics | CLU × Pomona",
    page_icon="⛳",
    layout="wide"
)

# =========================
# TEAM COLOR SYSTEM
# =========================
CLU_PURPLE  = "#7B2FBE"   # Cal Lutheran
POMONA_BLUE = "#1D6FE8"   # Pomona-Pitzer
WIN_GREEN   = "#00d980"   # Winner

TEAM_COLORS = {
    "cal lutheran": CLU_PURPLE,
    "clu":          CLU_PURPLE,
    "cal lu":       CLU_PURPLE,
    "pomona":       POMONA_BLUE,
    "pomona-pitzer":POMONA_BLUE,
    "pitzer":       POMONA_BLUE,
    "winner":       WIN_GREEN,
}

def get_team_color(team_name: str) -> str:
    key = str(team_name).lower().strip()
    for k, v in TEAM_COLORS.items():
        if k in key:
            return v
    fallback = ["#f5c842", "#ff4d6a", "#06b6d4", "#a78bfa"]
    return fallback[hash(team_name) % len(fallback)]

# =========================
# DESIGN SYSTEM CSS
# =========================
st.markdown("""
<style>
@import url('https://fonts.googleapis.com/css2?family=Space+Mono:wght@400;700&family=DM+Sans:wght@300;400;500;600&display=swap');

:root {
    --bg-void:       #000000;
    --bg-card:       #0d1117;
    --clu-purple:    #7B2FBE;
    --pomona-blue:   #1D6FE8;
    --win-green:     #00d980;
    --mono:          'Space Mono', monospace;
    --sans:          'DM Sans', sans-serif;
}

/* ── GLOBAL ── */
html, body, [class*="css"], .stApp {
    background-color: #000000 !important;
    color: #ffffff !important;
    font-family: var(--sans) !important;
}

/* ── FORCE BRIGHT WHITE ON ALL MAIN CONTENT TEXT ── */
.stApp p, .stApp span, .stApp div, .stApp li,
.stMarkdown p, .stMarkdown span, .stMarkdown div,
[data-testid="stMarkdownContainer"] p,
[data-testid="stMarkdownContainer"] span,
[data-testid="stMarkdownContainer"] div {
    color: #ffffff !important;
}

.stApp .stMarkdown * {
    color: #ffffff !important;
}

/* ── SIDEBAR — WHITE BACKGROUND ── */
section[data-testid="stSidebar"] {
    background-color: #ffffff !important;
    border-right: 1px solid rgba(0,0,0,0.10) !important;
}

/* Force ALL text in sidebar to dark/black — comprehensive override */
section[data-testid="stSidebar"],
section[data-testid="stSidebar"] *,
section[data-testid="stSidebar"] p,
section[data-testid="stSidebar"] span,
section[data-testid="stSidebar"] div,
section[data-testid="stSidebar"] li,
section[data-testid="stSidebar"] label,
section[data-testid="stSidebar"] h1,
section[data-testid="stSidebar"] h2,
section[data-testid="stSidebar"] h3,
section[data-testid="stSidebar"] h4,
section[data-testid="stSidebar"] .stMarkdown *,
section[data-testid="stSidebar"] [data-testid="stMarkdownContainer"] *,
section[data-testid="stSidebar"] [data-testid="stRadio"] *,
section[data-testid="stSidebar"] [data-testid="stSlider"] *,
section[data-testid="stSidebar"] [data-testid="stMultiSelect"] *,
section[data-testid="stSidebar"] .stRadio *,
section[data-testid="stSidebar"] .stSlider *,
section[data-testid="stSidebar"] .stMultiSelect * {
    color: #1a202c !important;
    font-family: var(--sans) !important;
}

section[data-testid="stSidebar"] h2,
section[data-testid="stSidebar"] h3 {
    font-family: var(--mono) !important;
    font-size: 0.7rem !important;
    letter-spacing: 0.18em !important;
    text-transform: uppercase !important;
    color: #1a202c !important;
}

section[data-testid="stSidebar"] label {
    color: #2d3748 !important;
    font-size: 0.78rem !important;
    font-weight: 600 !important;
    font-family: var(--mono) !important;
    letter-spacing: 0.10em !important;
    text-transform: uppercase !important;
}

/* ── SIDEBAR DROPDOWN / SELECT — WHITE BACKGROUND ── */
section[data-testid="stSidebar"] div[data-baseweb="select"] > div,
section[data-testid="stSidebar"] div[data-baseweb="select"],
section[data-testid="stSidebar"] [data-baseweb="select"] [class*="ValueContainer"],
section[data-testid="stSidebar"] [data-baseweb="select"] [class*="control"],
section[data-testid="stSidebar"] [data-baseweb="select"] [class*="container"] {
    background-color: #ffffff !important;
    border: 1.5px solid rgba(0,0,0,0.20) !important;
    border-radius: 6px !important;
    color: #1a202c !important;
}

section[data-testid="stSidebar"] div[data-baseweb="select"] > div:focus-within,
section[data-testid="stSidebar"] div[data-baseweb="select"]:focus-within {
    border-color: #7B2FBE !important;
    box-shadow: 0 0 0 2px rgba(123,47,190,0.15) !important;
}

/* Dropdown input text and placeholder */
section[data-testid="stSidebar"] [data-baseweb="select"] input,
section[data-testid="stSidebar"] [data-baseweb="select"] [class*="placeholder"],
section[data-testid="stSidebar"] [data-baseweb="select"] [class*="singleValue"],
section[data-testid="stSidebar"] [data-baseweb="select"] [class*="Input"] {
    color: #1a202c !important;
    background-color: transparent !important;
}

/* Dropdown arrow/chevron area */
section[data-testid="stSidebar"] [data-baseweb="select"] [class*="indicatorContainer"],
section[data-testid="stSidebar"] [data-baseweb="select"] svg {
    color: #4a5568 !important;
    fill: #4a5568 !important;
}

/* Multi-select tags */
section[data-testid="stSidebar"] [data-baseweb="tag"] {
    background-color: rgba(123,47,190,0.12) !important;
    border: 1px solid rgba(123,47,190,0.40) !important;
    border-radius: 4px !important;
}

section[data-testid="stSidebar"] [data-baseweb="tag"] span,
section[data-testid="stSidebar"] [data-baseweb="tag"] div,
section[data-testid="stSidebar"] [data-baseweb="tag"] * {
    color: #2d3748 !important;
    font-family: var(--mono) !important;
    font-size: 0.7rem !important;
}

/* Dropdown menu list (popover that appears below) */
[data-baseweb="popover"] [data-baseweb="menu"],
[data-baseweb="popover"] ul {
    background-color: #ffffff !important;
    border: 1px solid rgba(0,0,0,0.12) !important;
}

[data-baseweb="popover"] li,
[data-baseweb="popover"] [role="option"] {
    background-color: #ffffff !important;
    color: #1a202c !important;
}

[data-baseweb="popover"] li:hover,
[data-baseweb="popover"] [role="option"]:hover,
[data-baseweb="popover"] [aria-selected="true"] {
    background-color: rgba(123,47,190,0.08) !important;
    color: #7B2FBE !important;
}

/* ── SLIDER ── */
section[data-testid="stSidebar"] [role="slider"] {
    background-color: #7B2FBE !important;
    border-color: #7B2FBE !important;
}
section[data-testid="stSidebar"] [data-testid="stSliderTrack"] > div:nth-child(2) {
    background-color: #7B2FBE !important;
}
section[data-testid="stSidebar"] [data-testid="stSliderTrack"] {
    background-color: rgba(0,0,0,0.12) !important;
}

/* Slider value text */
section[data-testid="stSidebar"] [data-testid="stThumbValue"],
section[data-testid="stSidebar"] .stSlider p {
    color: #1a202c !important;
}

/* ── RADIO BUTTONS ── */
section[data-testid="stSidebar"] [data-testid="stRadio"] label,
section[data-testid="stSidebar"] [data-testid="stRadio"] p,
section[data-testid="stSidebar"] [data-testid="stRadio"] span,
section[data-testid="stSidebar"] .stRadio label {
    color: #1a202c !important;
    font-family: var(--sans) !important;
    font-size: 0.88rem !important;
}

section[data-testid="stSidebar"] hr {
    border-color: rgba(0,0,0,0.10) !important;
}

/* ── KPI CARDS ── */
[data-testid="metric-container"] {
    background: linear-gradient(135deg, #0d1117 0%, #0a0e18 100%) !important;
    border: 1px solid rgba(255,255,255,0.08) !important;
    border-radius: 12px !important;
    padding: 20px 24px !important;
    position: relative;
    overflow: hidden;
    transition: border-color 0.2s, box-shadow 0.2s;
}
[data-testid="metric-container"]:hover {
    border-color: rgba(255,255,255,0.18) !important;
    box-shadow: 0 0 28px rgba(123,47,190,0.18);
}
[data-testid="metric-container"]::before {
    content: '';
    position: absolute;
    top: 0; left: 0; right: 0;
    height: 2px;
    background: linear-gradient(90deg, transparent, #7B2FBE, #1D6FE8, transparent);
}
[data-testid="metric-container"] [data-testid="stMetricLabel"] {
    color: #a0aec0 !important;
    font-family: var(--mono) !important;
    font-size: 0.65rem !important;
    letter-spacing: 0.15em !important;
    text-transform: uppercase !important;
}
[data-testid="metric-container"] [data-testid="stMetricValue"] {
    color: #ffffff !important;
    font-family: var(--mono) !important;
    font-size: 1.9rem !important;
    font-weight: 700 !important;
}

/* ── HEADINGS ── */
h1, h2, h3 {
    font-family: var(--mono) !important;
    color: #ffffff !important;
    letter-spacing: -0.01em !important;
}
h1 { font-size: 1.6rem !important; }
h2 { font-size: 1.1rem !important; color: #a0aec0 !important; }
h3 {
    font-size: 0.75rem !important;
    color: #ffffff !important;
    letter-spacing: 0.15em !important;
    text-transform: uppercase !important;
    border-bottom: 1px solid rgba(255,255,255,0.07);
    padding-bottom: 8px;
    margin-bottom: 16px !important;
}

/* ── TABS ── */
.stTabs [data-baseweb="tab-list"] {
    background-color: transparent !important;
    border-bottom: 1px solid rgba(255,255,255,0.07) !important;
    gap: 4px;
}
.stTabs [data-baseweb="tab"] {
    background-color: transparent !important;
    color: #a0aec0 !important;
    font-family: var(--mono) !important;
    font-size: 0.72rem !important;
    letter-spacing: 0.1em !important;
    text-transform: uppercase !important;
    padding: 10px 20px !important;
    border: none !important;
    border-radius: 0 !important;
    border-bottom: 2px solid transparent !important;
    transition: all 0.2s !important;
}
.stTabs [data-baseweb="tab"]:hover {
    color: #ffffff !important;
    background-color: rgba(255,255,255,0.03) !important;
}
.stTabs [aria-selected="true"] {
    color: #ffffff !important;
    border-bottom: 2px solid #7B2FBE !important;
    background-color: transparent !important;
}
.stTabs [data-baseweb="tab-panel"] {
    padding-top: 24px !important;
}

/* ── ALERT / INFO ── */
.stAlert {
    background-color: #0d1117 !important;
    border: 1px solid rgba(255,255,255,0.08) !important;
    border-radius: 8px !important;
    color: #a0aec0 !important;
    font-family: var(--mono) !important;
    font-size: 0.75rem !important;
}

/* ── SCROLLBAR ── */
::-webkit-scrollbar { width: 4px; height: 4px; }
::-webkit-scrollbar-track { background: #000000; }
::-webkit-scrollbar-thumb { background: #2d3748; border-radius: 2px; }
::-webkit-scrollbar-thumb:hover { background: #7B2FBE; }

/* ── SIDEBAR COLLAPSE BUTTON — GOLF BALL ── */
@keyframes golfBounce {
    0%   { transform: translateY(0) rotate(0deg); }
    25%  { transform: translateY(-12px) rotate(-20deg); }
    50%  { transform: translateY(0) rotate(0deg); }
    75%  { transform: translateY(-6px) rotate(12deg); }
    100% { transform: translateY(0) rotate(0deg); }
}

button[data-testid="baseButton-headerNoPadding"],
button[kind="header"] {
    background: transparent !important;
    border: none !important;
    width: 36px !important;
    height: 52px !important;
    padding: 0 !important;
    display: flex !important;
    align-items: center !important;
    justify-content: center !important;
    cursor: pointer !important;
    position: relative !important;
}

button[data-testid="baseButton-headerNoPadding"] svg,
button[kind="header"] svg {
    display: none !important;
}

button[data-testid="baseButton-headerNoPadding"]::before,
button[kind="header"]::before {
    content: '';
    display: block;
    width: 36px;
    height: 52px;
    background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='36' height='52' viewBox='0 0 36 52'%3E%3Ccircle cx='18' cy='14' r='13' fill='%23111111' stroke='%23444' stroke-width='0.8'/%3E%3Ccircle cx='14' cy='11' r='1.5' fill='%232a2a2a'/%3E%3Ccircle cx='19' cy='9' r='1.5' fill='%232a2a2a'/%3E%3Ccircle cx='23' cy='13' r='1.5' fill='%232a2a2a'/%3E%3Ccircle cx='15' cy='17' r='1.5' fill='%232a2a2a'/%3E%3Ccircle cx='21' cy='17' r='1.5' fill='%232a2a2a'/%3E%3Ccircle cx='18' cy='22' r='1.3' fill='%232a2a2a'/%3E%3Cline x1='18' y1='27' x2='18' y2='47' stroke='%23c8860a' stroke-width='2.5' stroke-linecap='round'/%3E%3Cellipse cx='18' cy='27.5' rx='6' ry='2' fill='%23e09b1a'/%3E%3C/svg%3E");
    background-repeat: no-repeat;
    background-size: contain;
    background-position: center;
    transition: transform 0.2s;
}

button[data-testid="baseButton-headerNoPadding"]:active::before,
button[kind="header"]:active::before {
    animation: golfBounce 0.55s ease forwards;
}

button[data-testid="baseButton-headerNoPadding"]:hover::before,
button[kind="header"]:hover::before {
    transform: translateY(-3px);
}

hr { border-color: rgba(255,255,255,0.07) !important; margin: 24px 0 !important; }

/* ── HEADER STYLES ── */
.header-badge {
    display: inline-block;
    background: rgba(123,47,190,0.15);
    border: 1px solid rgba(123,47,190,0.4);
    color: #c084fc;
    font-family: var(--mono);
    font-size: 0.6rem;
    letter-spacing: 0.2em;
    text-transform: uppercase;
    padding: 3px 10px;
    border-radius: 3px;
    margin-bottom: 6px;
}
.header-title {
    font-family: 'Space Mono', monospace;
    font-size: 2rem;
    font-weight: 700;
    color: #ffffff;
    letter-spacing: -0.02em;
    line-height: 1.1;
    margin: 0;
}
.header-sub {
    font-family: 'DM Sans', sans-serif;
    font-size: 0.9rem;
    color: #ffffff !important;
    margin-top: 6px;
    font-weight: 300;
}
.divider-line {
    width: 100%;
    height: 1px;
    background: linear-gradient(90deg, #7B2FBE, #1D6FE8, transparent);
    margin: 20px 0 28px 0;
}
</style>
""", unsafe_allow_html=True)

# =========================
# CHART THEME FUNCTION
# =========================
def apply_chart_theme(fig, title=None):
    fig.update_layout(
        paper_bgcolor="#0a0a0a",
        plot_bgcolor="#0a0a0a",
        font=dict(family="Space Mono, monospace", color="#a0aec0", size=11),
        title=dict(text=title, font=dict(color="#ffffff", size=13, family="Space Mono"), x=0) if title else None,
        legend=dict(
            bgcolor="rgba(10,10,10,0.9)",
            bordercolor="rgba(255,255,255,0.10)",
            borderwidth=1,
            font=dict(color="#ffffff", size=10, family="Space Mono"),
        ),
        xaxis=dict(
            gridcolor="rgba(255,255,255,0.04)",
            linecolor="rgba(255,255,255,0.10)",
            tickfont=dict(color="#ffffff", size=9, family="Space Mono"),
            tickangle=-30,
            title_font=dict(color="#ffffff"),
        ),
        yaxis=dict(
            gridcolor="rgba(255,255,255,0.04)",
            linecolor="rgba(255,255,255,0.10)",
            tickfont=dict(color="#ffffff", size=9, family="Space Mono"),
            zerolinecolor="rgba(255,255,255,0.12)",
            title_font=dict(color="#ffffff"),
        ),
        margin=dict(l=12, r=12, t=40 if title else 20, b=40),
        hovermode="x unified",
        hoverlabel=dict(
            bgcolor="#0d1117",
            bordercolor="rgba(255,255,255,0.18)",
            font=dict(color="#ffffff", size=11, family="Space Mono"),
        ),
    )
    return fig

# =========================
# LOAD DATA
# =========================
@st.cache_data
def load_data():
    df = pd.read_csv("golf_data_validated.csv")
    df["Event_clean"] = (
        df["Event name"].astype(str).str.strip()
        .str.replace(r"\s+", " ", regex=True).str.lower()
    )
    df["Team"] = df["Team"].astype(str).str.strip()
    df["score"] = pd.to_numeric(df["score"], errors="coerce")
    df["Winner Score"] = pd.to_numeric(df["Winner Score"], errors="coerce")
    df["Score_vs_Winner"] = df["score"] - df["Winner Score"]
    df = df.dropna(subset=["score", "Winner Score"])
    df["Points"] = (
        df["Weighted Points"]
        .astype(str)
        .str.extract(r"([\d.]+)")[0]
        .pipe(pd.to_numeric, errors="coerce")
    )
    return df

df = load_data()

# =========================
# SIDEBAR
# =========================
with st.sidebar:
    st.markdown("### ⚙ FILTERS")
    st.markdown("")

    team_options = ["Cal Lutheran", "Pomona Pitzer"]
    team_choice = st.radio("Teams", team_options, index=0)
    teams = [team_choice]
    select_all_events = st.checkbox("Select All Tournaments", value=True)

    event_list = df["Event_clean"].unique()

    events = st.multiselect(
        "Tournaments",
        event_list,
        default=event_list if select_all_events else []
    )
    score_range = st.slider(
        "Score Range",
        int(df["score"].min()), int(df["score"].max()),
        (int(df["score"].min()), int(df["score"].max()))
    )
    st.markdown("---")
    select_all_teams = st.checkbox("Select All Teams (H2H)", value=False)

    team_list = df["Team"].unique()

    compare_teams = st.multiselect(
        "Head-to-Head (select 2)",
        team_list,
        default=team_list if select_all_teams else [],
        max_selections=2
    )
    st.markdown("")
    st.markdown("""
    <div style="font-family:'Space Mono',monospace;font-size:0.62rem;color:#1a202c;letter-spacing:0.1em;text-transform:uppercase;margin-bottom:8px;">Color Key</div>
    <div style="display:flex;flex-direction:column;gap:7px;">
      <div style="display:flex;align-items:center;gap:8px;">
        <span style="display:inline-block;width:11px;height:11px;border-radius:50%;background:#7B2FBE;flex-shrink:0;"></span>
        <span style="font-family:'Space Mono',monospace;font-size:0.65rem;color:#1a202c;">Cal Lutheran</span>
      </div>
      <div style="display:flex;align-items:center;gap:8px;">
        <span style="display:inline-block;width:11px;height:11px;border-radius:50%;background:#1D6FE8;flex-shrink:0;"></span>
        <span style="font-family:'Space Mono',monospace;font-size:0.65rem;color:#1a202c;">Pomona-Pitzer</span>
      </div>
      <div style="display:flex;align-items:center;gap:8px;">
        <span style="display:inline-block;width:11px;height:11px;border-radius:50%;background:#00d980;flex-shrink:0;"></span>
        <span style="font-family:'Space Mono',monospace;font-size:0.65rem;color:#1a202c;">Winner</span>
      </div>
    </div>
    """, unsafe_allow_html=True)

# =========================
# FILTER DATA
# =========================
filtered_df = df[
    (df["Team"].isin(teams)) &
    (df["Event_clean"].isin(events)) &
    (df["score"].between(score_range[0], score_range[1]))
]

pivot_score = filtered_df.pivot_table(
    index="Event_clean", columns="Team", values="score", aggfunc="mean"
).reset_index()

pivot_gap = filtered_df.pivot_table(
    index="Event_clean", columns="Team", values="Score_vs_Winner", aggfunc="mean"
).reset_index()

winner_series = filtered_df.groupby("Event_clean")["Winner Score"].mean().reset_index()
winner_series["Team"] = "Winner"
winner_series.rename(columns={"Winner Score": "score"}, inplace=True)

combined_scores = pd.concat([
    filtered_df[["Event_clean", "Team", "score"]],
    winner_series
])
combined_scores = combined_scores.groupby(["Event_clean", "Team"])["score"].mean().reset_index()

# =========================
# HEADER
# =========================
st.markdown("""
<div class="header-badge">NCAA DIII · Season Analytics</div>
<p class="header-title">⛳ Golf Performance<br>Intelligence</p>
<p class="header-sub" style="color:#ffffff;">Cal Lutheran × Pomona-Pitzer — Tournament data, trend analysis, and competitive benchmarking.</p>
<div class="divider-line"></div>
""", unsafe_allow_html=True)

# =========================
# KPI ROW
# =========================
TEAM_RANKINGS = {
    "cal lutheran": "#32",
    "pomona pitzer": "#24",
    "pomona-pitzer": "#24",
}

def get_ranking(selected_teams):
    if not selected_teams:
        return "—"
    rankings = []
    for t in selected_teams:
        key = t.lower().strip()
        for k, v in TEAM_RANKINGS.items():
            if k in key:
                rankings.append(v)
                break
    if len(rankings) == 1:
        return rankings[0]
    elif len(rankings) > 1:
        return " / ".join(rankings)
    return "—"

def get_team_names(selected_teams):
    if not selected_teams:
        return "—"
    return " & ".join(selected_teams) if len(selected_teams) > 1 else selected_teams[0]

col1, col2, col3, col4, col5 = st.columns(5)
col1.metric("Tournaments", filtered_df["Event_clean"].nunique())
col2.metric("Ranking", get_ranking(teams))

team_avg_points = (
    filtered_df.groupby("Team")["Points"].mean().round(1)
    if not filtered_df.empty else {}
)
avg_pts_display = " / ".join([str(v) for v in team_avg_points.values]) if len(team_avg_points) else "—"
col3.metric("Avg Points", avg_pts_display)
col4.metric("Team Name", get_team_names(teams))


st.markdown("<div style='margin-top:32px'></div>", unsafe_allow_html=True)

# =========================
# TABS
# =========================
tab1, tab2, tab3 = st.tabs(["◉  MAP", "◈  TRENDS", "◆  ANALYSIS"])

# ── MAP ─────────────────────────────────────────
with tab1:

    st.markdown("### Tournament Finishes by Location")

    if {"Latitude", "Longitude", "Position"}.issubset(df.columns):

        map_df = df.copy()
        map_df = map_df[
            (map_df["Team"].isin(teams)) &
            (map_df["Event_clean"].isin(events))
        ]

        map_df["Finish_Position"] = (
            map_df["Position"]
            .astype(str)
            .str.extract(r"(\d+)")[0]
        )
        map_df["Finish_Position"] = pd.to_numeric(map_df["Finish_Position"], errors="coerce")
        map_df = map_df.dropna(subset=["Finish_Position"])

        map_df = (
            map_df.groupby("Event_clean")
            .agg({
                "Latitude": "first",
                "Longitude": "first",
                "Finish_Position": "min"
            })
            .reset_index()
        )

        def ordinal(n):
            n = int(n)
            if 10 <= n % 100 <= 20:
                suffix = "th"
            else:
                suffix = {1: "st", 2: "nd", 3: "rd"}.get(n % 10, "th")
            return f"{n}{suffix}"

        map_df["Place_Label"] = map_df["Finish_Position"].apply(ordinal)

        def get_place_color(place):
            if place == 1: return "#FFD700"
            elif place == 2: return "#C0C0C0"
            elif place == 3: return "#CD7F32"
            else: return "#ff4d6a"

        map_df["Color"] = map_df["Finish_Position"].apply(get_place_color)

        fig_map = go.Figure()
        categories = [
            ("🥇 1st Place", 1, "#FFD700"),
            ("🥈 2nd Place", 2, "#C0C0C0"),
            ("🥉 3rd Place", 3, "#CD7F32"),
            ("Outside Top 3", 999, "#ff4d6a"),
        ]

        for label, place_val, color in categories:
            if place_val == 999:
                subset = map_df[map_df["Finish_Position"] > 3]
            else:
                subset = map_df[map_df["Finish_Position"] == place_val]
            if subset.empty:
                continue
            fig_map.add_trace(go.Scattermapbox(
                lat=subset["Latitude"], lon=subset["Longitude"],
                mode="markers", name=label,
                marker=dict(size=18, color=color, opacity=0.92),
                text=subset.apply(
                    lambda r: f"<b>{r['Event_clean'].title()}</b><br>Finish: {r['Place_Label']}",
                    axis=1
                ),
                hoverinfo="text"
            ))

        fig_map.update_layout(
            mapbox=dict(style="carto-darkmatter", zoom=3.2, center=dict(lat=37.5, lon=-119)),
            paper_bgcolor="#0a0a0a",
            margin=dict(l=0, r=0, t=0, b=0),
            legend=dict(
                bgcolor="rgba(10,10,10,0.85)",
                bordercolor="rgba(255,255,255,0.10)",
                borderwidth=1,
                font=dict(color="#ffffff", family="Space Mono", size=10)
            )
        )
        st.plotly_chart(fig_map, use_container_width=True)
    else:
        st.error("Dataset missing Latitude, Longitude, or Position columns.")

    st.markdown("### 🏆 SCIAC Rival Scoreboard")

    # =========================
    # BUILD SCOREBOARD DATA
    # =========================
    scoreboard_df = filtered_df[[
        "Event_clean",
        "Team",
        "score",
        "Winner Score",
        "Score_vs_Winner",
        "Points"
    ]].copy()

    # =========================
    # CLEAN + FORMAT VALUES
    # =========================
    scoreboard_df["score"] = pd.to_numeric(scoreboard_df["score"], errors="coerce").round(2)
    scoreboard_df["Winner Score"] = pd.to_numeric(scoreboard_df["Winner Score"], errors="coerce").round(2)
    scoreboard_df["Score_vs_Winner"] = pd.to_numeric(scoreboard_df["Score_vs_Winner"], errors="coerce")

    # Golf-style formatting (+ / -)
    scoreboard_df["Score_vs_Winner"] = scoreboard_df["Score_vs_Winner"].apply(
        lambda x: f"{x:+.2f}" if pd.notnull(x) else ""
    )

    # =========================
    # MASTERS-STYLE HTML TABLE
    # =========================
    html = """
    <style>
    table.masters {
        border-collapse: collapse;
        width: 100%;
        font-family: 'Space Mono', monospace;
        font-size: 13px;
    }

    /* HEADER (Masters Yellow) */
    table.masters th {
        background-color: #FFD700;
        color: #000000;
        padding: 10px;
        text-transform: uppercase;
        letter-spacing: 0.08em;
        border: 1px solid #000000;
    }

    /* BODY (Masters Green) */
    table.masters td {
        background-color: #0B6E4F;
        color: #ffffff;
        padding: 10px;
        border: 1px solid #064B36;
    }

    /* Hover highlight */
    table.masters tr:hover td {
        background-color: #0f8a62;
    }
    </style>
    """

    html += "<table class='masters'>"

    # =========================
    # HEADER ROW
    # =========================
    html += "<tr>"
    for col in scoreboard_df.columns:
        html += f"<th>{col}</th>"
    html += "</tr>"

    # =========================
    # DATA ROWS
    # =========================
    for _, row in scoreboard_df.iterrows():
        html += "<tr>"
        for col in scoreboard_df.columns:
            val = row[col]

            # Clean NaN display
            if pd.isna(val):
                val = ""

            html += f"<td>{val}</td>"
        html += "</tr>"

    html += "</table>"

    st.markdown(html, unsafe_allow_html=True)

# ── TRENDS ──────────────────────────────────────
# ── TRENDS ──────────────────────────────────────
# ── TRENDS ──────────────────────────────────────

with tab2:

    st.markdown("### Score Trajectory by Tournament")

    # =========================
    # TOURNAMENT AVERAGE DATA (SAFE VERSION)
    # =========================
    avg_df_raw = df[["Event_clean", "Tournament Average"]].copy()

    event_order = pivot_score["Event_clean"].astype(str).str.lower().tolist()

    avg_map = dict(zip(
        avg_df_raw["Event_clean"],
        avg_df_raw["Tournament Average"]
    ))

    avg_df = pd.DataFrame({
        "Event_clean": event_order,
        "Avg Score": [avg_map.get(e, None) for e in event_order]
    })

    avg_df["Avg Score"] = avg_df["Avg Score"].ffill()

    avg_df_raw["Event_clean"] = avg_df_raw["Event_clean"].astype(str).str.strip().str.lower()
    avg_df_raw["Tournament Average"] = pd.to_numeric(
        avg_df_raw["Tournament Average"],
        errors="coerce"
    )

    avg_df_raw = avg_df_raw.groupby("Event_clean", as_index=False)["Tournament Average"].mean()

    # =========================
    # ALIGN TO ACTUAL TOURNAMENT ORDER
    # =========================
    event_order = pivot_score["Event_clean"].tolist()

    avg_map = dict(zip(avg_df["Event_clean"], avg_df["Avg Score"]))

    avg_df = pd.DataFrame({
        "Event_clean": event_order,
        "Avg Score": [avg_map.get(e, None) for e in event_order]
    })

    # fallback fill so line NEVER breaks
    avg_df["Avg Score"] = avg_df["Avg Score"].ffill()

    # =========================
    # TEAM LINES
    # =========================
    fig_line = go.Figure()

    team_cols = [c for c in pivot_score.columns if c != "Event_clean"]

    for team in team_cols:
        color = get_team_color(team)

        fig_line.add_trace(go.Scatter(
            x=pivot_score["Event_clean"],
            y=pivot_score[team],
            name=team,
            mode="lines+markers",
            line=dict(color=color, width=2.5),
            marker=dict(size=8, color=color),
            hovertemplate=f"<b>{team}</b><br>%{{x}}<br>Score: %{{y:.1f}}<extra></extra>"
        ))

    # =========================
    # TOURNAMENT AVG LINE (FORCED VISIBILITY)
    # =========================
    fig_line.add_trace(go.Scatter(
        x=avg_df["Event_clean"],
        y=avg_df["Avg Score"],
        name="Tournament Avg",
        mode="lines",
        line=dict(color="rgba(255,255,255,0.75)", width=3, dash="dot"),
        connectgaps=True,
        hovertemplate="<b>Tournament Avg</b><br>%{x}<br>%{y:.1f}<extra></extra>"
    ))

    apply_chart_theme(fig_line)
    st.plotly_chart(fig_line, use_container_width=True)

    st.markdown("### 🏆 SCIAC Rival Scoreboard")

    # =========================
    # BUILD SCOREBOARD DATA
    # =========================
    scoreboard_df = filtered_df[[
        "Event_clean",
        "Team",
        "score",
        "Winner Score",
        "Score_vs_Winner",
        "Points"
    ]].copy()

    # =========================
    # CLEAN + FORMAT VALUES
    # =========================
    scoreboard_df["score"] = pd.to_numeric(scoreboard_df["score"], errors="coerce").round(2)
    scoreboard_df["Winner Score"] = pd.to_numeric(scoreboard_df["Winner Score"], errors="coerce").round(2)
    scoreboard_df["Score_vs_Winner"] = pd.to_numeric(scoreboard_df["Score_vs_Winner"], errors="coerce")

    # Golf-style formatting (+ / -)
    scoreboard_df["Score_vs_Winner"] = scoreboard_df["Score_vs_Winner"].apply(
        lambda x: f"{x:+.2f}" if pd.notnull(x) else ""
    )

    # =========================
    # MASTERS-STYLE HTML TABLE
    # =========================
    html = """
    <style>
    table.masters {
        border-collapse: collapse;
        width: 100%;
        font-family: 'Space Mono', monospace;
        font-size: 13px;
    }

    /* HEADER (Masters Yellow) */
    table.masters th {
        background-color: #FFD700;
        color: #000000;
        padding: 10px;
        text-transform: uppercase;
        letter-spacing: 0.08em;
        border: 1px solid #000000;
    }

    /* BODY (Masters Green) */
    table.masters td {
        background-color: #0B6E4F;
        color: #ffffff;
        padding: 10px;
        border: 1px solid #064B36;
    }

    /* Hover highlight */
    table.masters tr:hover td {
        background-color: #0f8a62;
    }
    </style>
    """

    html += "<table class='masters'>"

    # =========================
    # HEADER ROW
    # =========================
    html += "<tr>"
    for col in scoreboard_df.columns:
        html += f"<th>{col}</th>"
    html += "</tr>"

    # =========================
    # DATA ROWS
    # =========================
    for _, row in scoreboard_df.iterrows():
        html += "<tr>"
        for col in scoreboard_df.columns:
            val = row[col]

            # Clean NaN display
            if pd.isna(val):
                val = ""

            html += f"<td>{val}</td>"
        html += "</tr>"

    html += "</table>"

    st.markdown(html, unsafe_allow_html=True)

# ── ANALYSIS ────────────────────────────────────
with tab3:

    st.markdown("### Tournament Scores vs Winner")
    all_teams_bar = combined_scores["Team"].unique().tolist()
    color_map_bar = {t: get_team_color(t) for t in all_teams_bar}
    fig_bar = px.bar(
        combined_scores, x="Event_clean", y="score", color="Team",
        barmode="group",
        color_discrete_map=color_map_bar,
    )
    fig_bar.update_traces(marker_line_width=0)
    apply_chart_theme(fig_bar)
    st.plotly_chart(fig_bar, use_container_width=True)

    st.markdown("### Strokes Behind Winner")
    fig_gap = go.Figure()
    gap_cols = [c for c in pivot_gap.columns if c != "Event_clean"]
    for team in gap_cols:
        color = get_team_color(team)
        fig_gap.add_trace(go.Bar(
            x=pivot_gap["Event_clean"],
            y=pivot_gap[team],
            name=team,
            marker_color=color,
            marker_line_width=0,
            hovertemplate=f"<b>{team}</b><br>%{{x}}<br>+%{{y:.1f}} strokes<extra></extra>"
        ))
    fig_gap.add_hline(
        y=0, line_color=WIN_GREEN, line_width=2,
        line_dash="dot", annotation_text="WINNER",
        annotation_font=dict(color=WIN_GREEN, size=9, family="Space Mono"),
    )
    fig_gap.update_layout(barmode="group")
    apply_chart_theme(fig_gap)
    st.plotly_chart(fig_gap, use_container_width=True)

    # ── IMPROVED SCORE DISTRIBUTION ──────────────────
    st.markdown("### Score Distribution")

    teams_in_filter = filtered_df["Team"].unique().tolist()

    if not filtered_df.empty and len(teams_in_filter) > 0:

        # Layout: summary stats cards + violin chart side by side
        dist_col1, dist_col2 = st.columns([1, 2])

        with dist_col1:
            # Summary stats per team
            for team in teams_in_filter:
                team_data = filtered_df[filtered_df["Team"] == team]["score"]
                color = get_team_color(team)
                mean_val = team_data.mean()
                median_val = team_data.median()
                std_val = team_data.std()
                min_val = team_data.min()
                max_val = team_data.max()
                count_val = len(team_data)

                st.markdown(f"""
                <div style="
                    background: #0d1117;
                    border: 1px solid {color}44;
                    border-left: 3px solid {color};
                    border-radius: 8px;
                    padding: 14px 16px;
                    margin-bottom: 12px;
                ">
                    <div style="font-family:'Space Mono',monospace;font-size:0.62rem;color:{color};letter-spacing:0.15em;text-transform:uppercase;margin-bottom:10px;">{team}</div>
                    <div style="display:grid;grid-template-columns:1fr 1fr;gap:8px;">
                        <div>
                            <div style="font-family:'Space Mono',monospace;font-size:0.58rem;color:#718096;text-transform:uppercase;letter-spacing:0.1em;">Avg</div>
                            <div style="font-family:'Space Mono',monospace;font-size:1.1rem;color:#fff;font-weight:700;">{mean_val:.1f}</div>
                        </div>
                        <div>
                            <div style="font-family:'Space Mono',monospace;font-size:0.58rem;color:#718096;text-transform:uppercase;letter-spacing:0.1em;">Median</div>
                            <div style="font-family:'Space Mono',monospace;font-size:1.1rem;color:#fff;font-weight:700;">{median_val:.1f}</div>
                        </div>
                        <div>
                            <div style="font-family:'Space Mono',monospace;font-size:0.58rem;color:#718096;text-transform:uppercase;letter-spacing:0.1em;">Std Dev</div>
                            <div style="font-family:'Space Mono',monospace;font-size:1.0rem;color:#a0aec0;">±{std_val:.1f}</div>
                        </div>
                        <div>
                            <div style="font-family:'Space Mono',monospace;font-size:0.58rem;color:#718096;text-transform:uppercase;letter-spacing:0.1em;">Range</div>
                            <div style="font-family:'Space Mono',monospace;font-size:1.0rem;color:#a0aec0;">{min_val:.0f}–{max_val:.0f}</div>
                        </div>
                    </div>
                    <div style="margin-top:8px;font-family:'Space Mono',monospace;font-size:0.58rem;color:#4a5568;">{count_val} rounds recorded</div>
                </div>
                """, unsafe_allow_html=True)

        with dist_col2:
            # Violin plot with box + individual points
            fig_violin = go.Figure()

            for team in teams_in_filter:
                team_data = filtered_df[filtered_df["Team"] == team]["score"]
                color = get_team_color(team)

                # Violin shape
                fig_violin.add_trace(
                    go.Violin(
                        y=team_data,
                        name=team,
                        box_visible=True,
                        meanline_visible=True,
                        points="all",
                        jitter=0.25,
                        pointpos=0,
                        fillcolor=f'rgba({int(color[1:3], 16)}, {int(color[3:5], 16)}, {int(color[5:7], 16)}, 0.25)',
                        line_color=color,
                        marker=dict(
                            color=color,
                            size=6,
                            opacity=0.7,
                            line=dict(color="#000000", width=1)
                        ),
                        meanline=dict(color="#ffffff", width=2),
                        opacity=0.85,
                        hovertemplate=f"<b>{team}</b><br>Score: %{{y}}<extra></extra>",
                        width=0.5,
                    )
                )

            fig_violin.update_layout(
                paper_bgcolor="#0a0a0a",
                plot_bgcolor="#0a0a0a",
                violinmode="group",
                violingap=0.3,
                violingroupgap=0.1,
                showlegend=True,
                legend=dict(
                    bgcolor="rgba(10,10,10,0.9)",
                    bordercolor="rgba(255,255,255,0.10)",
                    borderwidth=1,
                    font=dict(color="#ffffff", size=10, family="Space Mono"),
                ),
                yaxis=dict(
                    title="Score",
                    gridcolor="rgba(255,255,255,0.05)",
                    linecolor="rgba(255,255,255,0.10)",
                    tickfont=dict(color="#ffffff", size=9, family="Space Mono"),
                    zerolinecolor="rgba(255,255,255,0.12)",
                    title_font=dict(color="#a0aec0", family="Space Mono", size=10),
                ),
                xaxis=dict(
                    gridcolor="rgba(255,255,255,0.04)",
                    linecolor="rgba(255,255,255,0.10)",
                    tickfont=dict(color="#ffffff", size=10, family="Space Mono"),
                    title_font=dict(color="#ffffff"),
                ),
                margin=dict(l=12, r=12, t=20, b=20),
                hovermode="closest",
                hoverlabel=dict(
                    bgcolor="#0d1117",
                    bordercolor="rgba(255,255,255,0.18)",
                    font=dict(color="#ffffff", size=11, family="Space Mono"),
                ),
                height=380,
            )

            st.plotly_chart(fig_violin, use_container_width=True)

            # Score percentile context strip
            st.markdown("""
            <div style="font-family:'Space Mono',monospace;font-size:0.6rem;color:#4a5568;letter-spacing:0.1em;text-transform:uppercase;margin-top:4px;">
            ○ Each dot = one round &nbsp;·&nbsp; Box = IQR &nbsp;·&nbsp; White line = mean &nbsp;·&nbsp; Width = score frequency
            </div>
            """, unsafe_allow_html=True)

    else:
        st.info("No data available for the current filter selection.")

    st.markdown("### Head-to-Head")

    if len(compare_teams) != 2:
        st.info("↑ Select 2 teams in the sidebar to enable head-to-head comparison.")

    else:

        # Separate dataframe for comparison
        h2h_df = df[
            (df["Team"].isin(compare_teams)) &
            (df["Event_clean"].isin(events)) &
            (df["score"].between(score_range[0], score_range[1]))
            ]

        pivot_h2h = h2h_df.pivot_table(
            index="Event_clean",
            columns="Team",
            values="Score_vs_Winner",
            aggfunc="mean"
        ).reset_index()

        team_a, team_b = compare_teams

        if team_a not in pivot_h2h.columns or team_b not in pivot_h2h.columns:
            st.warning("No overlapping tournament data found for selected teams.")

        else:

            fig_h2h = go.Figure()

            for team in [team_a, team_b]:
                fig_h2h.add_trace(
                    go.Bar(
                        x=pivot_h2h["Event_clean"],
                        y=pivot_h2h[team],
                        name=team,
                        marker_color=get_team_color(team),
                        marker_line_width=0,
                        hovertemplate=f"<b>{team}</b><br>%{{x}}<br>%{{y:.1f}} strokes<extra></extra>"
                    )
                )

            fig_h2h.add_hline(
                y=0,
                line_color=WIN_GREEN,
                line_width=2,
                line_dash="dot",
                annotation_text="WINNER"
            )

            fig_h2h.update_layout(barmode="group")

            apply_chart_theme(fig_h2h)

            st.plotly_chart(fig_h2h, use_container_width=True)

# =========================
# FOOTER
# =========================
st.markdown("""
<div style="margin-top:48px; padding-top:20px; border-top:1px solid rgba(255,255,255,0.06);">
  <span style="font-family:'Space Mono',monospace; font-size:0.65rem; color:#4a5568; letter-spacing:0.15em; text-transform:uppercase;">
    College Golf Analytics · NCAA DIII · Cal Lutheran × Pomona-Pitzer · Built for performance evaluation
  </span>
</div>
""", unsafe_allow_html=True)

