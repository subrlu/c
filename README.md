<!DOCTYPE html>
<html lang="ja">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>計算機 - Material Design 3</title>
<link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;500;700&family=Noto+Sans+JP:wght@400;500;700&display=swap" rel="stylesheet">
<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Material+Symbols+Rounded:opsz,wght,FILL,GRAD@24,400,0,0" />
<style>
:root {
  --primary: #0061A4;
  --on-primary: #ffffff;
  --primary-container: #D1E4FF;
  --on-primary-container: #001D36;
  --surface: #F8F9FF;
  --on-surface: #191C20;
  --surface-variant: transparent; 
  --on-surface-variant: #44474E;
  --outline: #74777F;
  --bg: #E7F0F8;
  --radius: 24px;
}
* { box-sizing: border-box; }
body {
  font-family: "Roboto", "Noto Sans JP", sans-serif;
  background-color: var(--bg);
  margin: 0;
  padding: 0;
  color: var(--on-surface);
  display: flex;
  flex-direction: column;
  height: 100vh;
  -webkit-tap-highlight-color: transparent;
  overflow: hidden;
}
/* ===== Google Style Side Drawer ===== */
.menu-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.3);
  opacity: 0;
  visibility: hidden;
  transition: opacity 0.4s cubic-bezier(0.4, 0, 0.2, 1);
  z-index: 2000;
}
.menu {
  position: fixed;
  top: 0;
  left: -280px;
  width: 280px;
  height: 100%;
  background: #F3F6FC;
  box-shadow: 0 0 16px rgba(0,0,0,0.1);
  padding: 0 12px 100px 12px;
  display: flex;
  flex-direction: column;
  transition: transform 0.4s cubic-bezier(0.4, 0, 0.2, 1);
  z-index: 2001;
  overflow-y: auto;
  border-top-right-radius: 28px;
  border-bottom-right-radius: 28px;
}
.menu-title {
  padding: 24px 16px;
  font-size: 20px;
  font-weight: 500;
  color: var(--on-surface);
  cursor: pointer;
}
.menu.active { transform: translateX(280px); }
.menu-overlay.active { opacity: 1; visibility: visible; }
.menu a, .menu-label {
  display: flex;
  align-items: center;
  padding: 12px 16px;
  font-size: 14px;
  text-decoration: none;
  color: var(--on-surface-variant);
  border-radius: 100px;
  margin-bottom: 2px;
  opacity: 0;
  transform: translateY(-20px); 
  transition: opacity 0.5s cubic-bezier(0.4, 0, 0.2, 1), transform 0.5s cubic-bezier(0.4, 0, 0.2, 1);
}
.menu a { cursor: pointer; }
.menu-label {
  font-weight: 700;
  margin-top: 20px;
  color: var(--primary);
  pointer-events: none;
}
.menu.active a, .menu.active .menu-label {
  opacity: 1;
  transform: translateY(0);
}
.menu.active a:nth-child(n), .menu.active div:nth-child(n) { 
  transition-delay: calc(0.03s * var(--i)); 
}
.menu a:hover { background: rgba(0,0,0,0.05); }
.menu a.selected {
  background: var(--primary-container);
  color: var(--on-primary-container);
  font-weight: 500;
}
.menu .material-symbols-rounded, .menu .pi-text {
  font-size: 22px;
  margin-right: 12px;
  color: var(--primary);
  width: 24px;
  display: flex;
  justify-content: center;
}
.pi-text { font-family: serif; font-weight: bold; font-size: 20px; }
/* ===== Apps Grid Menu (Improved Animation) ===== */
.apps-container {
  position: fixed;
  top: 24px;
  right: 24px;
  z-index: 1100;
}
.apps-btn {
  width: 48px;
  height: 48px;
  background: none;
  border: none;
  cursor: pointer;
  display: grid;
  place-items: center;
  padding: 0;
  transition: background 0.2s, transform 0.2s cubic-bezier(0.175, 0.885, 0.32, 1.275);
  border-radius: 50%;
}
.apps-btn:hover { background: rgba(0, 0, 0, 0.05); }
.apps-btn:active { transform: scale(0.9); }
.apps-grid {
  display: grid;
  grid-template-columns: repeat(3, 4px);
  gap: 3px;
}
.apps-grid span {
  width: 4px;
  height: 4px;
  background: var(--on-surface-variant);
  border-radius: 50%;
}
.apps-menu {
  position: absolute;
  top: 60px;
  right: 0;
  width: 300px;
  max-height: 380px; 
  overflow-y: auto;
  background: #F3F6FC;
  border-radius: 20px;
  box-shadow: 0 4px 15px rgba(0,0,0,0.1);
  padding: 16px;
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 12px;
  opacity: 0;
  pointer-events: none;
  transform-origin: top right;
  transform: scale(0.9) translateY(20px);
  transition: opacity 0.3s cubic-bezier(0.4, 0, 0.2, 1), transform 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  z-index: 1000;
}
.apps-menu.active {
  opacity: 1;
  pointer-events: auto;
  transform: scale(1) translateY(0);
}
.apps-menu a {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 12px 4px;
  font-size: 11px;
  text-decoration: none;
  color: var(--on-surface-variant);
  border-radius: 12px;
  text-align: center;
  opacity: 0;
  transform: translateY(10px);
  transition: background 0.2s, opacity 0.4s ease, transform 0.4s ease;
}
.apps-menu.active a {
  opacity: 1;
  transform: translateY(0);
}
.apps-menu.active a:nth-child(n) { transition-delay: calc(0.05s * var(--i)); }
.apps-menu a:hover { background: rgba(0,0,0,0.05); }
.apps-menu .material-symbols-rounded {
  font-size: 28px;
  margin-bottom: 8px;
  color: var(--primary);
}
.apps-menu .pi-text {
  font-size: 24px;
  margin-bottom: 8px;
  color: var(--primary);
  font-family: serif;
  font-weight: 500;
}
/* ===== Settings Icon Button (left of apps button) ===== */
.settings-icon-btn {
  position: fixed;
  top: 24px;
  right: 84px;
  width: 48px;
  height: 48px;
  background: none;
  border: none;
  cursor: pointer;
  display: flex;
  justify-content: center;
  align-items: center;
  border-radius: 50%;
  z-index: 1100;
  transition: background 0.2s;
  color: var(--on-surface-variant);
}
.settings-icon-btn:hover { background: rgba(0, 0, 0, 0.05); }
.settings-icon-btn:active { transform: scale(0.9); }

/* ===== Memo Icon Button (desktop: left of split button; mobile: left of settings button) ===== */
.memo-icon-btn {
  position: fixed;
  top: 24px;
  right: 180px; /* desktop: left of split-btn (132px) by 48px */
  width: 48px;
  height: 48px;
  background: none;
  border: none;
  cursor: pointer;
  display: flex;
  justify-content: center;
  align-items: center;
  border-radius: 50%;
  z-index: 1100;
  transition: background 0.2s;
  color: var(--on-surface-variant);
}
.memo-icon-btn:hover { background: rgba(0, 0, 0, 0.05); }
.memo-icon-btn:active { transform: scale(0.9); }

/* ===== Answer Display Toggle Button ===== */
.ans-display-btn {
  position: fixed;
  top: 24px;
  right: 228px;
  width: 48px;
  height: 48px;
  background: none;
  border: none;
  cursor: pointer;
  display: flex;
  justify-content: center;
  align-items: center;
  border-radius: 50%;
  z-index: 1100;
  transition: background 0.2s;
  color: var(--on-surface-variant);
  font-size: 13px;
  font-weight: 700;
  font-family: inherit;
}
.ans-display-btn:hover { background: rgba(0, 0, 0, 0.05); }
.ans-display-btn:active { transform: scale(0.9); }

/* ===== Settings Overlay Panel ===== */
.settings-overlay-panel {
  position: fixed;
  top: 80px;
  right: 84px;
  width: 300px;
  background: #F3F6FC;
  border-radius: 20px;
  box-shadow: 0 4px 15px rgba(0,0,0,0.1);
  opacity: 0;
  pointer-events: none;
  transform-origin: top right;
  transform: scale(0.9) translateY(20px);
  transition: opacity 0.3s cubic-bezier(0.4, 0, 0.2, 1), transform 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  z-index: 1100;
}
.settings-overlay-panel.active {
  opacity: 1;
  pointer-events: auto;
  transform: scale(1) translateY(0);
}
.settings-overlay-inner {
  padding: 20px;
}

/* ===== Memo Panel (desktop: split right side) ===== */
.memo-panel {
  width: 0;
  max-width: 0;
  display: flex;
  flex-direction: column;
  opacity: 0;
  pointer-events: none;
  transform: scale(0.8) translateX(-100px);
  transition: all 0.6s cubic-bezier(0.34, 1.56, 0.64, 1);
  overflow: hidden;
}
.memo-panel.active {
  opacity: 1;
  pointer-events: auto;
  transform: scale(1);
  width: 100%;
  max-width: 480px;
  overflow: visible;
}
.memo-panel-inner {
  padding: 16px;
  display: flex;
  flex-direction: column;
  gap: 12px;
  height: 100%;
}
.memo-panel-title {
  font-size: 1em;
  font-weight: 500;
  color: var(--on-surface-variant);
  padding: 0 4px;
}
.memo-textarea {
  flex: 1;
  min-height: 300px;
  padding: 16px;
  border: 1.5px solid var(--outline);
  border-radius: var(--radius);
  background: var(--surface);
  color: var(--on-surface);
  font-size: 1em;
  font-family: inherit;
  outline: none;
  resize: none;
  transition: border-color 0.2s;
  line-height: 1.6;
}
.memo-textarea:focus {
  border-color: var(--primary);
}
.memo-clear-btn {
  padding: 12px 0;
  border: none;
  border-radius: 100px;
  background: rgba(0,0,0,0.07);
  color: var(--on-surface-variant);
  font-size: 0.9em;
  font-weight: 500;
  cursor: pointer;
  font-family: inherit;
  transition: background 0.2s;
}
.memo-clear-btn:active { opacity: 0.7; }

/* ===== Memo Overlay Panel (mobile) ===== */
.memo-overlay-panel {
  position: fixed;
  top: 80px;
  right: 132px;
  width: 300px;
  background: #F3F6FC;
  border-radius: 20px;
  box-shadow: 0 4px 15px rgba(0,0,0,0.1);
  opacity: 0;
  pointer-events: none;
  transform-origin: top right;
  transform: scale(0.9) translateY(20px);
  transition: opacity 0.3s cubic-bezier(0.4, 0, 0.2, 1), transform 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  z-index: 1100;
  display: none; /* shown only on mobile via JS */
}
.memo-overlay-panel.active {
  opacity: 1;
  pointer-events: auto;
  transform: scale(1) translateY(0);
}
.memo-overlay-inner {
  padding: 16px;
  display: flex;
  flex-direction: column;
  gap: 10px;
}
.memo-overlay-textarea {
  width: 100%;
  min-height: 220px;
  padding: 12px;
  border: 1.5px solid var(--outline);
  border-radius: 16px;
  background: var(--surface);
  color: var(--on-surface);
  font-size: 0.95em;
  font-family: inherit;
  outline: none;
  resize: none;
  transition: border-color 0.2s;
  line-height: 1.6;
}
.memo-overlay-textarea:focus { border-color: var(--primary); }
.memo-overlay-clear-btn {
  padding: 10px 0;
  border: none;
  border-radius: 100px;
  background: rgba(0,0,0,0.07);
  color: var(--on-surface-variant);
  font-size: 0.9em;
  font-weight: 500;
  cursor: pointer;
  font-family: inherit;
}

/* ===== Calculator Layout ===== */
.calculator-container {
  width: 100%;
  flex: 1 1 0;
  min-height: 0;
  display: flex;
  flex-direction: row; 
  align-items: center;
  justify-content: center;
  gap: 0;
  transition: gap 0.6s cubic-bezier(0.34, 1.56, 0.64, 1);
  padding: 72px 24px 24px 24px;
}
.calculator {
  width: 100%;
  max-width: 480px; 
  display: flex;
  flex-direction: column;
  transition: all 0.6s cubic-bezier(0.34, 1.56, 0.64, 1);
  opacity: 1;
  transform: scale(1);
}
.calculator.hidden {
  width: 0;
  max-width: 0;
  opacity: 0;
  pointer-events: none;
  transform: scale(0.8) translateX(100px);
  margin: 0;
  overflow: hidden;
}
.display-row {
  display: flex;
  flex-direction: column;
  align-items: flex-end;
  justify-content: center;
  height: 120px;
  margin-bottom: 12px;
  padding: 0 16px;
  overflow: hidden;
}
.history-display {
  font-size: 1.2em;
  color: var(--on-surface-variant);
  min-height: 1.5em;
  width: 100%;
  text-align: right;
  overflow-x: auto;
  white-space: nowrap;
}
/* ===== Realtime expression preview ===== */
.realtime-expr-display {
  font-size: 0.78em;
  color: var(--primary);
  min-height: 1.2em;
  width: 100%;
  text-align: right;
  overflow-x: auto;
  white-space: nowrap;
  opacity: 0.75;
  font-style: italic;
}
.main-display {
  width: 100%;
  height: 60px;
  font-size: 3.5em;
  font-weight: 400;
  text-align: right;
  border: none;
  background: transparent;
  color: var(--on-surface);
  font-family: inherit;
  outline: none;
  direction: ltr;
  overflow-x: auto;
  -webkit-overflow-scrolling: touch;
}
/* ===== Input Calculator Panel ===== */
.input-calc-panel {
  width: 0;
  max-width: 0;
  display: flex;
  flex-direction: column;
  gap: 0;
  opacity: 0;
  pointer-events: none;
  transform: scale(0.8) translateX(-100px);
  transition: all 0.6s cubic-bezier(0.34, 1.56, 0.64, 1);
  overflow: hidden;
}
.input-calc-panel.active {
  opacity: 1;
  pointer-events: auto;
  transform: scale(1);
  width: 100%;
  max-width: 480px;
}
.input-calc-inner {
  padding: 16px;
  display: flex;
  flex-direction: column;
  gap: 16px;
}
.input-calc-display {
  background: var(--primary-container);
  border-radius: var(--radius);
  padding: 16px 20px;
  min-height: 100px;
  display: flex;
  flex-direction: column;
  gap: 8px;
  overflow: hidden;
}
.input-calc-expr {
  font-size: 1.1em;
  color: var(--on-surface-variant);
  word-break: break-all;
  min-height: 1.4em;
}
/* Realtime preview inside input-calc-display */
.input-calc-realtime {
  font-size: 0.82em;
  color: var(--primary);
  font-style: italic;
  opacity: 0.8;
  min-height: 1.2em;
  word-break: break-all;
}
.input-calc-result {
  font-size: 2em;
  font-weight: 500;
  color: var(--on-primary-container);
  overflow-x: auto;
  white-space: nowrap;
  -webkit-overflow-scrolling: touch;
  direction: ltr;
  text-align: left;
}
.input-calc-input-row {
  display: flex;
  gap: 10px;
  align-items: stretch;
}
.input-calc-field {
  flex: 1;
  padding: 14px 16px;
  border: 1.5px solid var(--outline);
  border-radius: var(--radius);
  background: transparent;
  color: var(--on-surface);
  font-size: 1.1em;
  font-family: inherit;
  outline: none;
  transition: border-color 0.2s;
}
.input-calc-field:focus {
  border-color: var(--primary);
}
.input-calc-btn {
  padding: 14px 20px;
  border: none;
  border-radius: var(--radius);
  background: var(--primary);
  color: var(--on-primary);
  font-size: 1.1em;
  font-weight: 500;
  cursor: pointer;
  font-family: inherit;
  transition: opacity 0.2s, transform 0.1s;
  white-space: nowrap;
}
.input-calc-btn:active {
  opacity: 0.8;
  transform: scale(0.97);
}
.input-calc-hint {
  font-size: 0.85em;
  color: var(--on-surface-variant);
  padding: 0 4px;
  line-height: 1.6;
}
.side-menu-btn {
  position: fixed;
  top: 24px;
  left: 24px;
  width: 48px;
  height: 48px;
  background: none;
  border: none;
  cursor: pointer;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  gap: 4px;
  border-radius: 50%;
  z-index: 1000;
  transition: background 0.2s;
}
.side-menu-btn span {
  width: 20px;
  height: 2px;
  background: var(--on-surface-variant);
  border-radius: 2px;
}
.split-btn {
  position: fixed;
  top: 24px;
  right: 132px;
  width: 48px;
  height: 48px;
  background: none;
  border: none;
  cursor: pointer;
  display: flex;
  justify-content: center;
  align-items: center;
  border-radius: 50%;
  z-index: 1000;
  transition: background 0.2s, transform 0.3s;
  color: var(--on-surface-variant);
}
.button-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-auto-rows: 72px; 
  grid-gap: 12px;
}
.calculator.scientific .button-grid {
  grid-template-columns: repeat(5, 1fr);
  grid-auto-rows: 60px;
}
button.calc-btn {
  font-size: 1.5em;
  font-weight: 500;
  border: none;
  border-radius: var(--radius);
  background-color: var(--primary-container);
  color: var(--on-primary-container);
  cursor: pointer;
  transition: background 0.3s, transform 0.1s cubic-bezier(0.4, 0, 0.2, 1), box-shadow 0.3s;
  display: flex;
  justify-content: center;
  align-items: center;
  user-select: none;
  position: relative;
  overflow: hidden;
}
.calculator.scientific button.calc-btn {
  font-size: 1.1em;
}
button.calc-btn:active { 
  transform: scale(0.95);
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}
/* Ripple Effect Styles */
.ripple {
  position: absolute;
  background: rgba(0, 0, 0, 0.12);
  border-radius: 50%;
  transform: scale(0);
  animation: ripple-animation 0.6s linear;
  pointer-events: none;
}
@keyframes ripple-animation {
  to {
    transform: scale(4);
    opacity: 0;
  }
}
.orange { background-color: #FFDDB3 !important; color: #291800 !important; }
.red { background-color: #FFDAD6 !important; color: #410002 !important; }
.green { background-color: var(--primary) !important; color: var(--on-primary) !important; }
.blue { background-color: #D1E4FF !important; color: #001D36 !important; }
.wide { grid-column: span 2; }
/* ===== Settings Panel (in calculator-container, for layout purposes only) ===== */
.settings-panel {
  display: none;
}
.settings-panel.active {
  display: none;
}
.settings-row {
  margin-bottom: 28px;
}
.settings-row-label {
  font-size: 0.95em;
  color: var(--on-surface-variant);
  margin-bottom: 12px;
  font-weight: 500;
  padding: 0 4px;
}
.settings-toggle-group {
  display: flex;
  gap: 12px;
}
.settings-toggle-btn {
  flex: 1;
  padding: 14px 0;
  border: 1.5px solid var(--outline);
  border-radius: var(--radius);
  background: transparent;
  color: var(--on-surface-variant);
  font-size: 1em;
  font-weight: 500;
  cursor: pointer;
  transition: background 0.2s, color 0.2s, border-color 0.2s;
  font-family: inherit;
}
.settings-toggle-btn.active {
  background: var(--primary-container);
  color: var(--on-primary-container);
  border-color: var(--primary-container);
  font-weight: 700;
}
.settings-switch-row {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 14px 16px;
  background: var(--primary-container);
  border-radius: var(--radius);
}
.settings-switch-label {
  font-size: 1em;
  color: var(--on-primary-container);
  font-weight: 500;
}
.settings-switch {
  position: relative;
  width: 52px;
  height: 28px;
}
.settings-switch input {
  opacity: 0;
  width: 0;
  height: 0;
}
.settings-switch-track {
  position: absolute;
  top: 0; left: 0; right: 0; bottom: 0;
  background: var(--outline);
  border-radius: 100px;
  cursor: pointer;
  transition: background 0.2s;
}
.settings-switch input:checked + .settings-switch-track {
  background: var(--primary);
}
.settings-switch-track::before {
  content: '';
  position: absolute;
  width: 20px;
  height: 20px;
  left: 4px;
  top: 4px;
  background: white;
  border-radius: 50%;
  transition: transform 0.2s;
}
.settings-switch input:checked + .settings-switch-track::before {
  transform: translateX(24px);
}
/* ===== Language selector in settings ===== */
.settings-lang-selector {
  position: relative;
}
.settings-lang-current {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 12px 16px;
  border: 1.5px solid var(--outline);
  border-radius: 100px;
  background: var(--primary-container);
  color: var(--on-primary-container);
  font-size: 0.95em;
  font-weight: 700;
  cursor: pointer;
  font-family: inherit;
  transition: background 0.2s;
  width: 100%;
}
.settings-lang-current:hover { background: #c0d8f8; }
.settings-lang-current-arrow {
  font-size: 18px;
  transition: transform 0.25s cubic-bezier(0.4,0,0.2,1);
  color: var(--primary);
  display: flex;
  align-items: center;
}
.settings-lang-selector.open .settings-lang-current-arrow {
  transform: rotate(180deg);
}
.settings-lang-dropdown {
  position: absolute;
  left: 0;
  right: 0;
  top: calc(100% + 6px);
  background: #F3F6FC;
  border-radius: 16px;
  box-shadow: 0 4px 16px rgba(0,0,0,0.13);
  overflow: hidden;
  max-height: 0;
  opacity: 0;
  pointer-events: none;
  transition: max-height 0.3s cubic-bezier(0.4,0,0.2,1), opacity 0.25s;
  z-index: 10;
}
.settings-lang-selector.open .settings-lang-dropdown {
  max-height: 320px;
  opacity: 1;
  pointer-events: auto;
}
.settings-lang-option {
  display: block;
  width: 100%;
  padding: 11px 16px;
  border: none;
  background: transparent;
  color: var(--on-surface-variant);
  font-size: 0.95em;
  font-weight: 500;
  cursor: pointer;
  text-align: left;
  font-family: inherit;
  transition: background 0.15s;
}
.settings-lang-option:hover { background: rgba(0,0,0,0.05); }
.settings-lang-option.active {
  background: var(--primary-container);
  color: var(--on-primary-container);
  font-weight: 700;
}
/* ===== Math Tools Panel ===== */
.math-tools-panel {
  width: 0;
  max-width: 0;
  display: flex;
  flex-direction: column;
  gap: 0;
  opacity: 0;
  pointer-events: none;
  transform: scale(0.8) translateX(-100px);
  transition: all 0.6s cubic-bezier(0.34, 1.56, 0.64, 1);
  overflow: hidden;
}
.math-tools-panel.active {
  opacity: 1;
  pointer-events: auto;
  transform: scale(1);
  width: 100%;
  max-width: 480px;
  overflow-y: auto;
}
.math-tools-inner {
  padding: 16px;
  display: flex;
  flex-direction: column;
  gap: 16px;
}
/* Math Tools Tabs */
.math-tabs-container {
  display: flex;
  justify-content: center;
  margin-bottom: 8px;
  position: sticky;
  top: 0;
  z-index: 10;
  background: var(--bg);
  padding: 4px 0;
}
.math-tabs {
  display: inline-flex;
  background: var(--primary-container);
  padding: 4px;
  border-radius: 32px;
  gap: 4px;
}
.math-tab {
  padding: 8px 16px;
  border: none;
  background: none;
  border-radius: 28px;
  font-size: 13px;
  font-weight: 500;
  color: var(--on-primary-container);
  cursor: pointer;
  transition: background 0.3s, color 0.3s;
  font-family: inherit;
}
.math-tab.active {
  background: var(--primary);
  color: var(--on-primary);
}
.math-tab-content { display: none; }
.math-tab-content.active { display: block; }
/* Math Tools Card */
.math-card {
  background: var(--primary-container);
  border-radius: var(--radius);
  padding: 20px;
  margin-bottom: 8px;
}
.math-card h3 {
  margin-top: 0;
  font-size: 1.1em;
  color: var(--on-primary-container);
  margin-bottom: 14px;
}
.math-input-group {
  display: flex;
  gap: 8px;
  margin-bottom: 10px;
}
.math-number-input {
  flex: 1;
  padding: 12px 14px;
  font-size: 14px;
  border-radius: 16px;
  border: 1.5px solid var(--outline);
  background: var(--surface);
  color: var(--on-surface);
  outline: none;
  transition: border 0.2s;
  font-family: inherit;
}
.math-number-input:focus {
  border: 2px solid var(--primary);
}
.math-icon-btn {
  width: 48px;
  height: 48px;
  border-radius: 14px;
  border: none;
  background: var(--primary);
  color: white;
  cursor: pointer;
  display: grid;
  place-items: center;
  flex-shrink: 0;
  font-family: inherit;
}
.math-icon-btn .material-symbols-rounded {
  font-size: 22px;
  color: white;
}
.math-remove-btn {
  width: 48px;
  height: 48px;
  background: #FFDAD6;
  color: #410002;
  border-radius: 14px;
  border: none;
  cursor: pointer;
  display: grid;
  place-items: center;
  flex-shrink: 0;
}
.math-remove-btn .material-symbols-rounded {
  font-size: 22px;
  color: #410002;
}
.math-action-btn {
  width: 100%;
  margin-top: 10px;
  padding: 14px;
  background: var(--primary);
  color: white;
  border: none;
  border-radius: 100px;
  font-size: 14px;
  font-weight: 700;
  cursor: pointer;
  transition: opacity 0.2s, transform 0.1s;
  font-family: inherit;
}
.math-action-btn:active { transform: scale(0.98); }
.math-action-btn.add {
  background: rgba(0,0,0,0.08);
  color: var(--on-primary-container);
  margin-top: 6px;
  padding: 10px;
  border-radius: 14px;
}
.math-result {
  margin-top: 16px;
  padding: 14px 16px;
  background: rgba(255,255,255,0.7);
  border-radius: 14px;
  border-left: 4px solid var(--primary);
  font-weight: 500;
  color: var(--on-surface);
  font-size: 14px;
  line-height: 1.5;
  display: none;
}
.math-result.visible { display: block; }
/* ===== フリックポップアップ ===== */
.flick-popup {
  position: fixed;
  display: none;
  z-index: 9999;
  pointer-events: none;
}
.flick-popup-inner {
  display: flex;
  gap: 8px;
  background: rgba(30,30,40,0.92);
  border-radius: 16px;
  padding: 10px 18px;
  box-shadow: 0 4px 16px rgba(0,0,0,0.18);
}
.flick-popup-item {
  color: #fff;
  font-size: 1.5em;
  font-weight: 500;
  min-width: 36px;
  text-align: center;
  padding: 4px 8px;
  border-radius: 8px;
  background: transparent;
  transition: background 0.15s;
}
.flick-popup-item.active {
  background: var(--primary);
}
/* ===== タブレット: 数学ツールパネルを中央表示 ===== */
@media (min-width: 601px) and (max-width: 1024px) {
  .math-tools-panel {
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -40%) scale(0.8);
    width: calc(100% - 32px) !important;
    max-width: 480px !important;
    margin: 0;
    z-index: 500;
    overflow: visible;
  }
  .math-tools-panel.active {
    transform: translate(-50%, -50%) scale(1);
    width: calc(100% - 32px) !important;
    max-width: 480px !important;
    opacity: 1;
    max-height: 80vh;
    overflow-y: auto;
  }
  .input-calc-panel {
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -40%) scale(0.8);
    width: calc(100% - 32px) !important;
    max-width: 480px !important;
    margin: 0;
    z-index: 500;
    overflow: visible;
  }
  .input-calc-panel.active {
    transform: translate(-50%, -50%) scale(1);
    width: calc(100% - 32px) !important;
    max-width: 480px !important;
    opacity: 1;
  }
  .settings-overlay-panel {
    right: 50%;
    transform: translate(50%, 0) scale(0.9);
    transform-origin: top center;
  }
  .settings-overlay-panel.active {
    transform: translate(50%, 0) scale(1);
  }
}
@media (max-width: 600px) {
  .calculator-container { 
    padding: 72px 16px 16px 16px;
    align-items: center;
    justify-content: center;
    flex-direction: column; 
    gap: 0 !important;
    overflow-y: auto;
  } 
  .calculator {
    margin-top: auto;
  }
  .calculator.hidden { transform: scale(0.8) translateY(100px); }
  .display-row { height: 110px; margin-bottom: 8px; }
  .main-display { font-size: 2.8em; text-align: right; } 
  .button-grid { grid-gap: 8px; grid-auto-rows: 60px; } 
  .calculator.scientific .button-grid { grid-gap: 6px; grid-auto-rows: 50px; }
  button.calc-btn { font-size: 1.4em; border-radius: 16px; }
  .calculator.scientific button.calc-btn { font-size: 1em; }
  .split-btn { display: none; }
  /* On mobile, memo btn is left of settings btn */
  .memo-icon-btn {
    right: 132px; /* left of settings-icon-btn (84px) */
  }
  /* ans-display-btn hidden on mobile to avoid clutter */
  .ans-display-btn {
    display: none;
  }
  
  .apps-menu {
    position: fixed;
    top: 80px;
    left: 16px;
    right: 16px;
    width: auto;
    transform-origin: center top;
  }
  .settings-overlay-panel {
    position: fixed;
    top: 80px;
    left: 16px;
    right: 16px;
    width: auto;
    transform-origin: center top;
  }
  .settings-overlay-panel.active {
    transform: scale(1) translateY(0);
  }
  .memo-overlay-panel {
    display: block;
    position: fixed;
    top: 80px;
    left: 16px;
    right: 16px;
    width: auto;
    transform-origin: center top;
  }
  .memo-overlay-panel.active {
    transform: scale(1) translateY(0);
  }
  .input-calc-panel {
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -40%) scale(0.8);
    width: calc(100% - 32px) !important;
    max-width: calc(100% - 32px) !important;
    margin: 0;
    z-index: 500;
    overflow: visible;
  }
  .input-calc-panel.active {
    transform: translate(-50%, -50%) scale(1);
    width: calc(100% - 32px) !important;
    max-width: calc(100% - 32px) !important;
    opacity: 1;
  }
  .math-tools-panel {
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -40%) scale(0.8);
    width: calc(100% - 32px) !important;
    max-width: calc(100% - 32px) !important;
    margin: 0;
    z-index: 500;
    overflow: visible;
  }
  .math-tools-panel.active {
    transform: translate(-50%, -50%) scale(1);
    width: calc(100% - 32px) !important;
    max-width: calc(100% - 32px) !important;
    opacity: 1;
    max-height: 80vh;
    overflow-y: auto;
  }
  .settings-toggle-btn { font-size: 1.1em; }
  .settings-lang-dropdown {
    top: auto;
    bottom: calc(100% + 6px);
  }
}
</style>
</head>
<body>
<!-- フリックポップアップ（.ボタン用） -->
<div class="flick-popup" id="flickPopup">
  <div class="flick-popup-inner">
    <div class="flick-popup-item" id="flickLeft">（</div>
    <div class="flick-popup-item" id="flickCenter">.</div>
    <div class="flick-popup-item" id="flickRight">）</div>
  </div>
</div>
<!-- フリックポップアップ（x²ボタン用） -->
<div class="flick-popup" id="flickPopupCaret">
  <div class="flick-popup-inner">
    <div class="flick-popup-item" id="flickCaretLeft">/</div>
    <div class="flick-popup-item" id="flickCaretCenter">x²</div>
    <div class="flick-popup-item" id="flickCaretRight">!</div>
  </div>
</div>
<!-- フリックポップアップ（3ボタン用） -->
<div class="flick-popup" id="flickPopup3">
  <div class="flick-popup-inner">
    <div class="flick-popup-item" id="flick3Left">√</div>
    <div class="flick-popup-item" id="flick3Center">3</div>
    <div class="flick-popup-item" id="flick3Right">π</div>
  </div>
</div>
<!-- フリックポップアップ（2ボタン用） -->
<div class="flick-popup" id="flickPopup2">
  <div class="flick-popup-inner">
    <div class="flick-popup-item" id="flick2Left">P</div>
    <div class="flick-popup-item" id="flick2Center">2</div>
    <div class="flick-popup-item" id="flick2Right">C</div>
  </div>
</div>
<div class="menu-overlay" id="menuOverlay" onclick="toggleMenu()"></div>
<nav class="menu" id="menu">
  <div class="menu-title" onclick="toggleMenu()" style="--i:1" id="menuTitle">計算機</div>
  
  <a id="menuNormal" onclick="setMode(1, 'normal'); toggleMenu();" class="selected" style="--i:2">
    <span class="material-symbols-rounded">calculate</span><span data-i18n="modeNormal">普通計算</span>
  </a>
  <a id="menuScientific" onclick="setMode(1, 'scientific'); toggleMenu();" style="--i:3">
    <span class="material-symbols-rounded">functions</span><span data-i18n="modeScientific">関数計算</span>
  </a>
  <a id="menuInputCalc" onclick="setMode(1, 'inputcalc'); toggleMenu();" style="--i:4">
    <span class="material-symbols-rounded">keyboard</span><span data-i18n="modeInputCalc">入力計算</span>
  </a>
  <a id="menuMathTools" onclick="setMode(1, 'mathtools'); toggleMenu();" style="--i:5">
    <span class="material-symbols-rounded">library_books</span><span data-i18n="modeMathTools">数学ツール</span>
  </a>
  <div style="height: 60px; min-height: 60px; --i:6"></div>
</nav>
<button class="side-menu-btn" onclick="toggleMenu()" aria-label="メニュー">
  <span></span><span></span><span></span>
</button>
<button class="split-btn" onclick="toggleSplitMode()" aria-label="画面分割">
  <span class="material-symbols-rounded" id="splitIcon">view_agenda</span>
</button>
<!-- Memo icon button -->
<button class="memo-icon-btn" id="memoIconBtn" onclick="toggleMemoPanel()" aria-label="メモ">
  <span class="material-symbols-rounded">edit_note</span>
</button>
<!-- Answer display toggle button (desktop only) -->
<button class="ans-display-btn" id="ansDisplayBtn" onclick="cycleAnswerDisplayMode()" title="解答表示切替">1.5</button>
<button class="settings-icon-btn" onclick="toggleSettingsPanel()" aria-label="設定">
  <span class="material-symbols-rounded">settings</span>
</button>
<!-- Settings Overlay Panel (floats above, like apps-menu) -->
<div class="settings-overlay-panel" id="settingsOverlayPanel">
  <div class="settings-overlay-inner">
    <div class="settings-row">
      <div class="settings-row-label" data-i18n="settingsAngleUnit">角度単位</div>
      <div class="settings-toggle-group">
        <button class="settings-toggle-btn active" id="btnDEG" onclick="setAngleUnit('DEG')">DEG</button>
        <button class="settings-toggle-btn" id="btnRAD" onclick="setAngleUnit('RAD')">RAD</button>
      </div>
    </div>
    <div class="settings-row">
      <div class="settings-row-label" data-i18n="settingsThousands">千の位区切り表示</div>
      <div class="settings-switch-row">
        <span class="settings-switch-label" data-i18n="settingsThousandsLabel">千の位区切り</span>
        <label class="settings-switch">
          <input type="checkbox" id="thousandsSepToggle" onchange="setThousandsSep(this.checked)">
          <span class="settings-switch-track"></span>
        </label>
      </div>
    </div>
      <div class="settings-row" style="margin-bottom:0">
      <div class="settings-row-label" data-i18n="settingsLanguage">言語</div>
      <div class="settings-lang-selector" id="langSelector">
        <button class="settings-lang-current" id="langCurrentBtn" onclick="toggleLangDropdown()">
          <span id="langCurrentLabel">日本語</span>
          <span class="settings-lang-current-arrow material-symbols-rounded">expand_more</span>
        </button>
        <div class="settings-lang-dropdown" id="langDropdown"></div>
      </div>
    </div>
  </div>
</div>
<!-- Memo overlay panel (mobile only, shown via CSS) -->
<div class="memo-overlay-panel" id="memoOverlayPanel">
  <div class="memo-overlay-inner">
    <div class="settings-row-label">メモ</div>
    <textarea class="memo-overlay-textarea" id="memoOverlayTextarea" placeholder="メモを入力..."></textarea>
    <button class="memo-overlay-clear-btn" onclick="clearMemo()">クリア</button>
  </div>
</div>
<div class="apps-container" id="appsContainer">
  <button class="apps-btn" onclick="toggleAppsMenu()" aria-label="アプリメニュー">
    <div class="apps-grid">
      <span></span><span></span><span></span>
      <span></span><span></span><span></span>
      <span></span><span></span><span></span>
    </div>
  </button>
  <nav class="apps-menu" id="appsMenu">
    <a href="https://apps.subaru.now" target="_blank" style="--i:1"><span class="material-symbols-rounded">grid_view</span><span data-i18n="appList">アプリ一覧</span></a>
    <a href="https://scrive.subaru.now" target="_blank" style="--i:2"><span class="material-symbols-rounded">description</span><span data-i18n="appDocs">文書作成</span></a>
    <a href="https://unit.subaru.now" target="_blank" style="--i:3"><span class="material-symbols-rounded">straighten</span><span data-i18n="appUnit">単位変換</span></a>
    <a href="https://counts.subaru.now" target="_blank" style="--i:4"><span class="material-symbols-rounded">reorder</span><span data-i18n="appWordCount">文字数カウント</span></a>
    <a href="https://calculator.subaru.now" target="_blank" style="--i:5"><span class="material-symbols-rounded">calculate</span><span data-i18n="appCalc">計算機</span></a>
    <a href="https://translate.subaru.now" target="_blank" style="--i:6"><span class="material-symbols-rounded">translate</span><span data-i18n="appTranslate">翻訳</span></a>
    <a href="https://passwords.subaru.now" target="_blank" style="--i:7"><span class="material-symbols-rounded">key</span><span data-i18n="appPassword">パスワード生成</span></a>
    <a href="https://citysearch.subaru.now" target="_blank" style="--i:8"><span class="material-symbols-rounded">location_city</span><span data-i18n="appCitySearch">市町村検索</span></a>
    <a href="https://calendar.subaru.now" target="_blank" style="--i:9"><span class="material-symbols-rounded">calendar_month</span><span data-i18n="appCalendar">カレンダー</span></a>
    <a href="https://math.subaru.now" target="_blank" style="--i:10"><span class="material-symbols-rounded">functions</span><span data-i18n="appMath">計算ツール</span></a>
    <a href="https://colorpicker.subaru.now" target="_blank" style="--i:11"><span class="material-symbols-rounded">colorize</span><span data-i18n="appColorPicker">カラーピッカー</span></a>
    <a href="https://edit.subaru.now" target="_blank" style="--i:12"><span class="material-symbols-rounded">edit_square</span><span data-i18n="appPhotoEdit">写真編集</span></a>
    <a href="https://games.subaru.now" target="_blank" style="--i:13"><span class="material-symbols-rounded">sports_esports</span><span data-i18n="appGames">ゲーム</span></a>
    <a href="https://health.subaru.now" target="_blank" style="--i:14"><span class="material-symbols-rounded">ecg_heart</span><span data-i18n="appHealth">健康指標</span></a>
    <a href="https://device.subaru.now" target="_blank" style="--i:15"><span class="material-symbols-rounded">devices</span><span data-i18n="appDevice">端末情報</span></a>
    <a href="https://search.subaru.now" target="_blank" style="--i:16"><span class="material-symbols-rounded">search</span><span data-i18n="appSearch">検索</span></a>
    <a href="https://whiteboard.subaru.now" target="_blank" style="--i:17"><span class="material-symbols-rounded">gesture</span><span data-i18n="appWhiteboard">ホワイトボード</span></a>
    <a href="https://notes.subaru.now" target="_blank" style="--i:18"><span class="material-symbols-rounded">sticky_note_2</span><span data-i18n="appNotes">メモ</span></a>
    <a href="https://music.subaru.now/piano" target="_blank" style="--i:19"><span class="material-symbols-rounded">piano</span><span data-i18n="appPiano">ピアノ</span></a>
    <a href="https://clock.subaru.now" target="_blank" style="--i:20"><span class="material-symbols-rounded">schedule</span><span data-i18n="appClock">時計</span></a>
    <a href="https://domains.subaru.now/search" target="_blank" style="--i:21"><span class="material-symbols-rounded">link_2</span><span data-i18n="appDomainSearch">ドメイン検索</span></a>
    <a href="https://domains.subaru.now" target="_blank" style="--i:22"><span class="material-symbols-rounded">search_gear</span><span data-i18n="appTldSearch">TLD検索</span></a>
    <a href="https://math.subaru.now/pi" target="_blank" style="--i:23"><span class="pi-text">π</span><span data-i18n="appPiTest">円周率テスト</span></a>
    <a href="https://developers.subaru.now" target="_blank" style="--i:24"><span class="material-symbols-rounded">code</span><span data-i18n="appDevApps">試験運用中アプリ</span></a>
    <a href="https://math.subaru.now/payment" target="_blank" style="--i:25"><span class="material-symbols-rounded">currency_yen</span><span data-i18n="appSplit">割り勘計算</span></a>
    <a href="https://device.subaru.now/mouse" target="_blank" style="--i:26"><span class="material-symbols-rounded">mouse</span><span data-i18n="appMouse">マウス反応</span></a>
    <a href="https://counts.subaru.now/app" target="_blank" style="--i:27"><span class="material-symbols-rounded">list_alt_add</span><span data-i18n="appMultiCounter">マルチカウンタ</span></a>
    <a href="https://calendar.subaru.now/calculation" target="_blank" style="--i:28"><span class="material-symbols-rounded">calendar_clock</span><span data-i18n="appDateCalc">日付計算</span></a>
    <a href="https://sports.subaru.now/olympics" target="_blank" style="--i:29"><span class="material-symbols-rounded">social_leaderboard</span><span data-i18n="appOlympics">オリンピック開催地</span></a>
    <a href="https://sports.subaru.now/japangames" target="_blank" style="--i:30"><span class="material-symbols-rounded">rewarded_ads</span><span data-i18n="appJapanGames">国スポ開催地</span></a>
    <a href="https://ai.subaru.now" target="_blank" style="--i:31"><span class="material-symbols-rounded">emoji_objects</span><span data-i18n="appAI">AI</span></a>
    <a href="https://games.subaru.now/play/dice" target="_blank" style="--i:32"><span class="material-symbols-rounded">ifl</span><span data-i18n="appDice">サイコロ</span></a>
    <a href="https://weather.subaru.now/custom" target="_blank" style="--i:33"><span class="material-symbols-rounded">brightness_5</span><span data-i18n="appWeatherCustom">カスタム天気</span></a>
    <a href="https://music.subaru.now/custom" target="_blank" style="--i:34"><span class="material-symbols-rounded">queue_music</span><span data-i18n="appMusicCustom">カスタム楽譜</span></a>
    <a href="https://ratiochecker.subaru.now/" target="_blank" style="--i:35"><span class="material-symbols-rounded">aspect_ratio</span><span data-i18n="appRatio">画像比率</span></a>
    <a href="https://notes.subaru.now/code" target="_blank" style="--i:36"><span class="material-symbols-rounded">code</span><span data-i18n="appCodeMemo">コードメモ</span></a>
    <a href="https://ideas.subaru.now" target="_blank" style="--i:37"><span class="material-symbols-rounded">lightbulb_2</span><span data-i18n="appIdeas">アイデアボード</span></a>
    <a href="https://images.subaru.now/qrcode" target="_blank" style="--i:38"><span class="material-symbols-rounded">qr_code</span><span data-i18n="appQrCode">QRコード</span></a>
    <a href="https://search.subaru.now/country" target="_blank" style="--i:39"><span class="material-symbols-rounded">globe_location_pin</span><span data-i18n="appCountry">国・首都</span></a>
    <a href="https://images.subaru.now/url" target="_blank" style="--i:40"><span class="material-symbols-rounded">image_search</span><span data-i18n="appUrlImage">URL画像</span></a>
  </nav>
</div>
<div class="calculator-container" id="calcContainer">
  <div class="calculator" id="calc1">
    <div class="display-row">
      <div id="history1" class="history-display"></div>
      <div id="realtimeExpr1" class="realtime-expr-display"></div>
      <input type="text" id="display1" class="main-display" readonly placeholder="0">
    </div>
    <div class="button-grid" id="grid1">
      <button class="calc-btn" onclick="appendNumber('7', 1)">7</button>
      <button class="calc-btn" onclick="appendNumber('8', 1)">8</button>
      <button class="calc-btn" id="btn3_1" onclick="appendNumber('9', 1)">9</button>
      <button class="calc-btn orange" onclick="appendOperator('÷','/', 1)">÷</button>
      <button class="calc-btn" onclick="appendNumber('4', 1)">4</button>
      <button class="calc-btn" onclick="appendNumber('5', 1)">5</button>
      <button class="calc-btn" onclick="appendNumber('6', 1)">6</button>
      <button class="calc-btn orange" onclick="appendOperator('×','*', 1)">×</button>
      <button class="calc-btn" onclick="appendNumber('1', 1)">1</button>
      <button class="calc-btn" id="btn2_1" onclick="appendNumber('2', 1)">2</button>
      <button class="calc-btn" id="btn3num_1" onclick="appendNumber('3', 1)">3</button>
      <button class="calc-btn orange" onclick="appendOperator('-','-', 1)">−</button>
      <button class="calc-btn" onclick="appendNumber('0', 1)">0</button>
      <button class="calc-btn" id="dotBtn1" onclick="appendNumber('.', 1)">.</button>
      <button class="calc-btn" id="caretBtn1" onclick="appendCaretOperator(1)">x²</button>
      <button class="calc-btn orange" onclick="appendOperator('+','+', 1)">+</button>
      <button class="calc-btn red" onclick="clearDisplay(1)">C</button>
      <button class="calc-btn red" onclick="backspace(1)">⌫</button>
      <button class="calc-btn wide green" onclick="calculate(1)">=</button>
    </div>
  </div>
  <div class="calculator hidden" id="calc2">
    <div class="display-row">
      <div id="history2" class="history-display"></div>
      <div id="realtimeExpr2" class="realtime-expr-display"></div>
      <input type="text" id="display2" class="main-display" readonly placeholder="0">
    </div>
    <div class="button-grid" id="grid2">
      <button class="calc-btn" onclick="appendNumber('7', 2)">7</button>
      <button class="calc-btn" onclick="appendNumber('8', 2)">8</button>
      <button class="calc-btn" id="btn3_2" onclick="appendNumber('9', 2)">9</button>
      <button class="calc-btn orange" onclick="appendOperator('÷','/', 2)">÷</button>
      <button class="calc-btn" onclick="appendNumber('4', 2)">4</button>
      <button class="calc-btn" onclick="appendNumber('5', 2)">5</button>
      <button class="calc-btn" onclick="appendNumber('6', 2)">6</button>
      <button class="calc-btn orange" onclick="appendOperator('×','*', 2)">×</button>
      <button class="calc-btn" onclick="appendNumber('1', 2)">1</button>
      <button class="calc-btn" id="btn2_2" onclick="appendNumber('2', 2)">2</button>
      <button class="calc-btn" id="btn3num_2" onclick="appendNumber('3', 2)">3</button>
      <button class="calc-btn orange" onclick="appendOperator('-','-', 2)">−</button>
      <button class="calc-btn" onclick="appendNumber('0', 2)">0</button>
      <button class="calc-btn" id="dotBtn2" onclick="appendNumber('.', 2)">.</button>
      <button class="calc-btn" id="caretBtn2" onclick="appendCaretOperator(2)">x²</button>
      <button class="calc-btn orange" onclick="appendOperator('+','+', 2)">+</button>
      <button class="calc-btn red" onclick="clearDisplay(2)">C</button>
      <button class="calc-btn red" onclick="backspace(2)">⌫</button>
      <button class="calc-btn wide green" onclick="calculate(2)">=</button>
    </div>
  </div>
  <!-- Memo panel (desktop split mode) -->
  <div class="memo-panel" id="memoPanel">
    <div class="memo-panel-inner">
      <div class="memo-panel-title">メモ</div>
      <textarea class="memo-textarea" id="memoPanelTextarea" placeholder="メモを入力..."></textarea>
      <button class="memo-clear-btn" onclick="clearMemo()">クリア</button>
    </div>
  </div>
  <div class="input-calc-panel" id="inputCalcPanel">
    <div class="input-calc-inner">
      <div class="input-calc-display">
        <div class="input-calc-expr" id="inputCalcExpr">—</div>
        <div class="input-calc-realtime" id="inputCalcRealtime"></div>
        <div class="input-calc-result" id="inputCalcResult">—</div>
      </div>
      <div class="input-calc-input-row">
        <input
          type="text"
          id="inputCalcField"
          class="input-calc-field"
          placeholder="例: 1+2*3, sin(30), 10/3"
          oninput="inputCalcRealtimeUpdate(1)"
          onkeydown="if(event.key==='Enter') inputCalcEvaluate()"
        >
        <button class="input-calc-btn" onclick="inputCalcEvaluate()">=</button>
      </div>
      <div class="input-calc-hint">
       
      </div>
    </div>
  </div>
  <div class="input-calc-panel" id="inputCalcPanel2">
    <div class="input-calc-inner">
      <div class="input-calc-display">
        <div class="input-calc-expr" id="inputCalcExpr2">—</div>
        <div class="input-calc-realtime" id="inputCalcRealtime2"></div>
        <div class="input-calc-result" id="inputCalcResult2">—</div>
      </div>
      <div class="input-calc-input-row">
        <input
          type="text"
          id="inputCalcField2"
          class="input-calc-field"
          placeholder="例: 1+2*3, sin(30), 10/3"
          oninput="inputCalcRealtimeUpdate(2)"
          onkeydown="if(event.key==='Enter') inputCalcEvaluate2()"
        >
        <button class="input-calc-btn" onclick="inputCalcEvaluate2()">=</button>
      </div>
      <div class="input-calc-hint">
        使用可能: <code>+</code> <code>-</code> <code>*</code> <code>×</code> <code>/</code> <code>÷</code> <code>**</code> <code>^</code> <code>²</code> <code>³</code> <code>¹⁴⁵⁶⁷⁸⁹</code> <code>⁺⁻ˣᐟ‧⁽⁾</code> <code>( )</code> <code>sin</code> <code>cos</code> <code>tan</code> <code>sqrt</code> <code>log</code> <code>abs</code> <code>PI</code> <code>π</code> <code>E</code> <code>!</code><br>
        角度単位は設定の DEG / RAD に従います。
      </div>
    </div>
  </div>
  <div class="settings-panel" id="settingsPanel"></div>
  <!-- Math Tools Panel -->
  <div class="math-tools-panel" id="mathToolsPanel">
    <div class="math-tools-inner">
      <div class="math-tabs-container">
        <div class="math-tabs">
          <button class="math-tab active" onclick="openMathTab(0)" data-i18n="mathTabPrime">素因数分解</button>
          <button class="math-tab" onclick="openMathTab(1)" data-i18n="mathTabGcd">公約数</button>
          <button class="math-tab" onclick="openMathTab(2)" data-i18n="mathTabLcm">公倍数</button>
        </div>
      </div>
      <!-- 素因数分解 -->
      <div class="math-tab-content active" id="mathTab0">
        <div class="math-card">
          <h3 data-i18n="mathTabPrime">素因数分解</h3>
          <div class="math-input-group">
            <input type="number" id="primeInput" class="math-number-input" min="2" placeholder="数字を入力">
            <button class="math-icon-btn" onclick="primeFactor()">
              <span class="material-symbols-rounded">search</span>
            </button>
          </div>
          <div class="math-result" id="primeResult"></div>
        </div>
      </div>
      <!-- 公約数 -->
      <div class="math-tab-content" id="mathTab1">
        <div class="math-card">
          <h3 data-i18n="mathTabGcd">公約数</h3>
          <div id="gcdInputs"></div>
          <button class="math-action-btn add" onclick="addMathInput('gcdInputs')" data-i18n="mathAddNumber">数を追加</button>
          <button class="math-action-btn" onclick="commonDivisors()" data-i18n="mathSearch">調べる</button>
          <div class="math-result" id="gcdResult"></div>
        </div>
      </div>
      <!-- 公倍数 -->
      <div class="math-tab-content" id="mathTab2">
        <div class="math-card">
          <h3 data-i18n="mathTabLcm">公倍数</h3>
          <div id="lcmInputs"></div>
          <button class="math-action-btn add" onclick="addMathInput('lcmInputs')" data-i18n="mathAddNumber">数を追加</button>
          <button class="math-action-btn" onclick="commonMultiples()" data-i18n="mathSearch">調べる</button>
          <div class="math-result" id="lcmResult"></div>
        </div>
      </div>
    </div>
  </div>
</div>
<script>
// ===== i18n =====
const I18N = {
  ja: {
    pageTitle: '計算機',
    menuTitle: '計算機',
    modeNormal: '普通計算',
    modeScientific: '関数計算',
    modeInputCalc: '入力計算',
    modeMathTools: '数学ツール',
    settingsAngleUnit: '角度単位',
    settingsThousands: '千の位区切り表示',
    settingsThousandsLabel: '千の位区切り',
    settingsLanguage: '言語',
    mathTabPrime: '素因数分解',
    mathTabGcd: '公約数',
    mathTabLcm: '公倍数',
    mathAddNumber: '数を追加',
    mathSearch: '調べる',
    mathResultPrime: '結果',
    mathResultGcd: '公約数',
    mathResultLcm: '最小公倍数',
    inputCalcPlaceholder: '例: 1+2*3, sin(30), 10/3',
    inputCalcError: 'エラー',
    appList: 'アプリ一覧',
    appDocs: '文書作成',
    appUnit: '単位変換',
    appWordCount: '文字数カウント',
    appCalc: '計算機',
    appTranslate: '翻訳',
    appPassword: 'パスワード生成',
    appCitySearch: '市町村検索',
    appCalendar: 'カレンダー',
    appMath: '計算ツール',
    appColorPicker: 'カラーピッカー',
    appPhotoEdit: '写真編集',
    appGames: 'ゲーム',
    appHealth: '健康指標',
    appDevice: '端末情報',
    appSearch: '検索',
    appWhiteboard: 'ホワイトボード',
    appNotes: 'メモ',
    appPiano: 'ピアノ',
    appClock: '時計',
    appDomainSearch: 'ドメイン検索',
    appTldSearch: 'TLD検索',
    appPiTest: '円周率テスト',
    appDevApps: '試験運用中アプリ',
    appSplit: '割り勘計算',
    appMouse: 'マウス反応',
    appMultiCounter: 'マルチカウンタ',
    appDateCalc: '日付計算',
    appOlympics: 'オリンピック開催地',
    appJapanGames: '国スポ開催地',
    appAI: 'AI',
    appDice: 'サイコロ',
    appWeatherCustom: 'カスタム天気',
    appMusicCustom: 'カスタム楽譜',
    appRatio: '画像比率',
    appCodeMemo: 'コードメモ',
    appIdeas: 'アイディアボード',
    appQrCode: 'QRコード',
    appCountry: '国・首都',
    appUrlImage: 'URL画像',
    langLabel: '日本語'
  },
  en: {
    pageTitle: 'Calculator',
    menuTitle: 'Calculator',
    modeNormal: 'Standard',
    modeScientific: 'Scientific',
    modeInputCalc: 'Text Input',
    modeMathTools: 'Math Tools',
    settingsAngleUnit: 'Angle Unit',
    settingsThousands: 'Thousands Separator',
    settingsThousandsLabel: 'Thousands Sep',
    settingsLanguage: 'Language',
    mathTabPrime: 'Factorize',
    mathTabGcd: 'GCD',
    mathTabLcm: 'LCM',
    mathAddNumber: 'Add Number',
    mathSearch: 'Calculate',
    mathResultPrime: 'Result',
    mathResultGcd: 'GCD',
    mathResultLcm: 'LCM',
    inputCalcPlaceholder: 'e.g. 1+2*3, sin(30), 10/3',
    inputCalcError: 'Error',
    appList: 'App List',
    appDocs: 'Documents',
    appUnit: 'Unit Convert',
    appWordCount: 'Word Count',
    appCalc: 'Calculator',
    appTranslate: 'Translate',
    appPassword: 'Password Gen',
    appCitySearch: 'City Search',
    appCalendar: 'Calendar',
    appMath: 'Math Tools',
    appColorPicker: 'Color Picker',
    appPhotoEdit: 'Photo Edit',
    appGames: 'Games',
    appHealth: 'Health',
    appDevice: 'Device Info',
    appSearch: 'Search',
    appWhiteboard: 'Whiteboard',
    appNotes: 'Notes',
    appPiano: 'Piano',
    appClock: 'Clock',
    appDomainSearch: 'Domain Search',
    appTldSearch: 'TLD Search',
    appPiTest: 'Pi Test',
    appDevApps: 'Beta Apps',
    appSplit: 'Bill Split',
    appMouse: 'Mouse Test',
    appMultiCounter: 'Multi Counter',
    appDateCalc: 'Date Calc',
    appOlympics: 'Olympics Host',
    appJapanGames: 'Japan Games',
    appAI: 'AI',
    appDice: 'Dice',
    appWeatherCustom: 'Custom Weather',
    appMusicCustom: 'Custom Score',
    appRatio: 'Image Ratio',
    appCodeMemo: 'Code Memo',
    appIdeas: 'Idea Board',
    appQrCode: 'QR Code',
    appCountry: 'Countries',
    appUrlImage: 'URL Image',
    langLabel: 'English'
  },
  es: {
    pageTitle: 'Calculadora',
    menuTitle: 'Calculadora',
    modeNormal: 'Estándar',
    modeScientific: 'Científica',
    modeInputCalc: 'Entrada de Texto',
    modeMathTools: 'Herramientas',
    settingsAngleUnit: 'Unidad de Ángulo',
    settingsThousands: 'Separador de Miles',
    settingsThousandsLabel: 'Sep. de Miles',
    settingsLanguage: 'Idioma',
    mathTabPrime: 'Factorizar',
    mathTabGcd: 'MCD',
    mathTabLcm: 'MCM',
    mathAddNumber: 'Agregar Número',
    mathSearch: 'Calcular',
    mathResultPrime: 'Resultado',
    mathResultGcd: 'MCD',
    mathResultLcm: 'MCM',
    inputCalcPlaceholder: 'ej: 1+2*3, sin(30), 10/3',
    inputCalcError: 'Error',
    appList: 'Lista de Apps',
    appDocs: 'Documentos',
    appUnit: 'Convertir Unidades',
    appWordCount: 'Contar Palabras',
    appCalc: 'Calculadora',
    appTranslate: 'Traducir',
    appPassword: 'Generar Contraseña',
    appCitySearch: 'Buscar Ciudad',
    appCalendar: 'Calendario',
    appMath: 'Herramientas Mat.',
    appColorPicker: 'Selector de Color',
    appPhotoEdit: 'Editar Foto',
    appGames: 'Juegos',
    appHealth: 'Salud',
    appDevice: 'Info Dispositivo',
    appSearch: 'Buscar',
    appWhiteboard: 'Pizarra',
    appNotes: 'Notas',
    appPiano: 'Piano',
    appClock: 'Reloj',
    appDomainSearch: 'Buscar Dominio',
    appTldSearch: 'Buscar TLD',
    appPiTest: 'Test de Pi',
    appDevApps: 'Apps Beta',
    appSplit: 'Dividir Cuenta',
    appMouse: 'Test Ratón',
    appMultiCounter: 'Multi Contador',
    appDateCalc: 'Calcular Fecha',
    appOlympics: 'Sede Olímpica',
    appJapanGames: 'Juegos Japón',
    appAI: 'IA',
    appDice: 'Dado',
    appWeatherCustom: 'Tiempo Personalizado',
    appMusicCustom: 'Partitura Personalizada',
    appRatio: 'Relación de Imagen',
    appCodeMemo: 'Memo de Código',
    appIdeas: 'Tablero de Ideas',
    appQrCode: 'Código QR',
    appCountry: 'Países',
    appUrlImage: 'Imagen URL',
    langLabel: 'Español'
  },
  ko: {
    pageTitle: '계산기',
    menuTitle: '계산기',
    modeNormal: '기본 계산',
    modeScientific: '공학용 계산',
    modeInputCalc: '텍스트 입력',
    modeMathTools: '수학 도구',
    settingsAngleUnit: '각도 단위',
    settingsThousands: '천 단위 구분',
    settingsThousandsLabel: '천 단위 구분',
    settingsLanguage: '언어',
    mathTabPrime: '소인수분해',
    mathTabGcd: '공약수',
    mathTabLcm: '공배수',
    mathAddNumber: '숫자 추가',
    mathSearch: '계산하기',
    mathResultPrime: '결과',
    mathResultGcd: '공약수',
    mathResultLcm: '최소공배수',
    inputCalcPlaceholder: '예: 1+2*3, sin(30), 10/3',
    inputCalcError: '오류',
    appList: '앱 목록',
    appDocs: '문서 작성',
    appUnit: '단위 변환',
    appWordCount: '글자 수 세기',
    appCalc: '계산기',
    appTranslate: '번역',
    appPassword: '비밀번호 생성',
    appCitySearch: '도시 검색',
    appCalendar: '달력',
    appMath: '계산 도구',
    appColorPicker: '색상 선택',
    appPhotoEdit: '사진 편집',
    appGames: '게임',
    appHealth: '건강 지표',
    appDevice: '기기 정보',
    appSearch: '검색',
    appWhiteboard: '화이트보드',
    appNotes: '메모',
    appPiano: '피아노',
    appClock: '시계',
    appDomainSearch: '도메인 검색',
    appTldSearch: 'TLD 검색',
    appPiTest: '원주율 테스트',
    appDevApps: '베타 앱',
    appSplit: '더치페이 계산',
    appMouse: '마우스 반응',
    appMultiCounter: '멀티 카운터',
    appDateCalc: '날짜 계산',
    appOlympics: '올림픽 개최지',
    appJapanGames: '일본 국민체전',
    appAI: 'AI',
    appDice: '주사위',
    appWeatherCustom: '커스텀 날씨',
    appMusicCustom: '커스텀 악보',
    appRatio: '이미지 비율',
    appCodeMemo: '코드 메모',
    appIdeas: '아이디어 보드',
    appQrCode: 'QR 코드',
    appCountry: '국가·수도',
    appUrlImage: 'URL 이미지',
    langLabel: '한국어'
  },
  fr: {
    pageTitle: 'Calculatrice',
    menuTitle: 'Calculatrice',
    modeNormal: 'Standard',
    modeScientific: 'Scientifique',
    modeInputCalc: 'Saisie Texte',
    modeMathTools: 'Outils Maths',
    settingsAngleUnit: 'Unité d\'Angle',
    settingsThousands: 'Séparateur Milliers',
    settingsThousandsLabel: 'Sép. Milliers',
    settingsLanguage: 'Langue',
    mathTabPrime: 'Factoriser',
    mathTabGcd: 'PGCD',
    mathTabLcm: 'PPCM',
    mathAddNumber: 'Ajouter Nombre',
    mathSearch: 'Calculer',
    mathResultPrime: 'Résultat',
    mathResultGcd: 'PGCD',
    mathResultLcm: 'PPCM',
    inputCalcPlaceholder: 'ex: 1+2*3, sin(30), 10/3',
    inputCalcError: 'Erreur',
    appList: 'Liste d\'Apps',
    appDocs: 'Documents',
    appUnit: 'Convertir Unités',
    appWordCount: 'Compter Mots',
    appCalc: 'Calculatrice',
    appTranslate: 'Traduire',
    appPassword: 'Générer MDP',
    appCitySearch: 'Chercher Ville',
    appCalendar: 'Calendrier',
    appMath: 'Outils Maths',
    appColorPicker: 'Sélect. Couleur',
    appPhotoEdit: 'Éditer Photo',
    appGames: 'Jeux',
    appHealth: 'Santé',
    appDevice: 'Info Appareil',
    appSearch: 'Rechercher',
    appWhiteboard: 'Tableau Blanc',
    appNotes: 'Notes',
    appPiano: 'Piano',
    appClock: 'Horloge',
    appDomainSearch: 'Chercher Domaine',
    appTldSearch: 'Chercher TLD',
    appPiTest: 'Test Pi',
    appDevApps: 'Apps Bêta',
    appSplit: 'Partager Addition',
    appMouse: 'Test Souris',
    appMultiCounter: 'Multi Compteur',
    appDateCalc: 'Calcul Date',
    appOlympics: 'Hôte Olympique',
    appJapanGames: 'Jeux Japon',
    appAI: 'IA',
    appDice: 'Dé',
    appWeatherCustom: 'Météo Personnalisée',
    appMusicCustom: 'Partition Personnalisée',
    appRatio: 'Ratio Image',
    appCodeMemo: 'Mémo Code',
    appIdeas: 'Tableau d\'Idées',
    appQrCode: 'Code QR',
    appCountry: 'Pays',
    appUrlImage: 'Image URL',
    langLabel: 'Français'
  },
  de: {
    pageTitle: 'Rechner',
    menuTitle: 'Rechner',
    modeNormal: 'Standard',
    modeScientific: 'Wissenschaftlich',
    modeInputCalc: 'Texteingabe',
    modeMathTools: 'Mathe-Werkzeuge',
    settingsAngleUnit: 'Winkeleinheit',
    settingsThousands: 'Tausendertrennzeichen',
    settingsThousandsLabel: 'Tausendertrennung',
    settingsLanguage: 'Sprache',
    mathTabPrime: 'Primfaktoren',
    mathTabGcd: 'GGT',
    mathTabLcm: 'KGV',
    mathAddNumber: 'Zahl Hinzufügen',
    mathSearch: 'Berechnen',
    mathResultPrime: 'Ergebnis',
    mathResultGcd: 'GGT',
    mathResultLcm: 'KGV',
    inputCalcPlaceholder: 'z.B.: 1+2*3, sin(30), 10/3',
    inputCalcError: 'Fehler',
    appList: 'App-Liste',
    appDocs: 'Dokumente',
    appUnit: 'Einheiten Umrechnen',
    appWordCount: 'Wörter Zählen',
    appCalc: 'Rechner',
    appTranslate: 'Übersetzen',
    appPassword: 'Passwort Gen.',
    appCitySearch: 'Stadt Suchen',
    appCalendar: 'Kalender',
    appMath: 'Mathe-Werkzeuge',
    appColorPicker: 'Farbwähler',
    appPhotoEdit: 'Foto Bearbeiten',
    appGames: 'Spiele',
    appHealth: 'Gesundheit',
    appDevice: 'Gerätinfo',
    appSearch: 'Suchen',
    appWhiteboard: 'Whiteboard',
    appNotes: 'Notizen',
    appPiano: 'Klavier',
    appClock: 'Uhr',
    appDomainSearch: 'Domain Suchen',
    appTldSearch: 'TLD Suchen',
    appPiTest: 'Pi Test',
    appDevApps: 'Beta Apps',
    appSplit: 'Rechnung Teilen',
    appMouse: 'Maus Test',
    appMultiCounter: 'Multi Zähler',
    appDateCalc: 'Datum Berechnen',
    appOlympics: 'Olympia Austragungsort',
    appJapanGames: 'Japan Spiele',
    appAI: 'KI',
    appDice: 'Würfel',
    appWeatherCustom: 'Benutzerdefiniertes Wetter',
    appMusicCustom: 'Benutzerdefinierte Noten',
    appRatio: 'Bildverhältnis',
    appCodeMemo: 'Code-Memo',
    appIdeas: 'Ideenboard',
    appQrCode: 'QR-Code',
    appCountry: 'Länder',
    appUrlImage: 'URL-Bild',
    langLabel: 'Deutsch'
  }
};
const LANG_LIST = [
  { code: 'ja', label: '日本語' },
  { code: 'en', label: 'English' },
  { code: 'es', label: 'Español' },
  { code: 'ko', label: '한국어' },
  { code: 'fr', label: 'Français' },
  { code: 'de', label: 'Deutsch' }
];
function detectLang() {
  const nav = navigator.language || navigator.userLanguage || 'en';
  const code = nav.toLowerCase().split('-')[0];
  if (I18N[code]) return code;
  return 'en';
}
let currentLang = detectLang();
function applyLang(lang) {
  if (!I18N[lang]) lang = 'en';
  currentLang = lang;
  const t = I18N[lang];
  document.title = t.pageTitle;
  document.getElementById('menuTitle').textContent = t.menuTitle;
  document.querySelectorAll('[data-i18n]').forEach(el => {
    const key = el.getAttribute('data-i18n');
    if (t[key] !== undefined) el.textContent = t[key];
  });
  // Update current lang label
  const currentLabel = document.getElementById('langCurrentLabel');
  if (currentLabel) currentLabel.textContent = t.langLabel || lang;
  // Update dropdown option active state
  document.querySelectorAll('.settings-lang-option').forEach(btn => {
    btn.classList.toggle('active', btn.dataset.lang === lang);
  });
  // Close dropdown
  const selector = document.getElementById('langSelector');
  if (selector) selector.classList.remove('open');
  // Update inputCalcField placeholders
  document.querySelectorAll('.input-calc-field').forEach(el => {
    el.placeholder = t.inputCalcPlaceholder;
  });
  // Update math number input placeholders
  document.querySelectorAll('.math-number-input').forEach(el => {
    el.placeholder = t.inputCalcPlaceholder.split(':')[1]?.trim().split(',')[0] || '2';
  });
}
function toggleLangDropdown() {
  const selector = document.getElementById('langSelector');
  selector.classList.toggle('open');
}
function buildLangButtons() {
  const dropdown = document.getElementById('langDropdown');
  dropdown.innerHTML = '';
  LANG_LIST.forEach(({ code, label }) => {
    const btn = document.createElement('button');
    btn.className = 'settings-lang-option' + (code === currentLang ? ' active' : '');
    btn.dataset.lang = code;
    btn.textContent = label;
    btn.onclick = () => applyLang(code);
    dropdown.appendChild(btn);
  });
  // Set current label
  const currentLabel = document.getElementById('langCurrentLabel');
  if (currentLabel) currentLabel.textContent = I18N[currentLang]?.langLabel || currentLang;
}
// ===== end i18n =====
function toggleMenu() {
  document.getElementById("menu").classList.toggle("active");
  document.getElementById("menuOverlay").classList.toggle("active");
}
function toggleAppsMenu() {
  const menu = document.getElementById("appsMenu");
  const isActive = menu.classList.toggle("active");
  if (isActive) {
    menu.scrollTop = 0;
    // Close settings if open
    document.getElementById('settingsOverlayPanel').classList.remove('active');
  }
}
document.addEventListener("click", (e) => {
  const menu = document.getElementById("appsMenu");
  if (!e.target.closest("#appsContainer")) {
    if (menu.classList.contains("active")) {
      menu.classList.remove("active");
    }
  }
  const settingsPanel = document.getElementById('settingsOverlayPanel');
  if (!e.target.closest('.settings-icon-btn') && !e.target.closest('#settingsOverlayPanel')) {
    if (settingsPanel.classList.contains('active')) {
      settingsPanel.classList.remove('active');
    }
  }
  // Close memo overlay if clicking outside (mobile)
  const memoOverlay = document.getElementById('memoOverlayPanel');
  if (!e.target.closest('.memo-icon-btn') && !e.target.closest('#memoOverlayPanel')) {
    if (memoOverlay.classList.contains('active')) {
      memoOverlay.classList.remove('active');
    }
  }
  // Close lang dropdown if clicking outside
  const langSelector = document.getElementById('langSelector');
  if (langSelector && !e.target.closest('#langSelector')) {
    langSelector.classList.remove('open');
  }
});
// ===== Settings State =====
let angleUnit = 'DEG';
let thousandsSep = false;

// ===== Answer Display Mode: 'decimal' | 'fraction' | 'scientific' =====
// cycles: decimal -> fraction -> scientific -> decimal
let answerDisplayMode = 'decimal';
// Labels for the button
const ANS_DISPLAY_LABELS = { decimal: '1.5', fraction: '½', scientific: '1e4' };

function cycleAnswerDisplayMode() {
  const modes = ['decimal', 'fraction', 'scientific'];
  const idx = modes.indexOf(answerDisplayMode);
  answerDisplayMode = modes[(idx + 1) % modes.length];
  const btn = document.getElementById('ansDisplayBtn');
  if (btn) btn.textContent = ANS_DISPLAY_LABELS[answerDisplayMode];
  // Re-display current results for both calculators
  [1, 2].forEach(id => {
    const s = state[id];
    if (s.isResultDisplayed && s.calcInput !== '') {
      const num = parseFloat(s.calcInput);
      if (!isNaN(num)) {
        const formatted = formatResultWithMode(num);
        s.displayInput = formatted;
        document.getElementById('display' + id).value = formatted;
      }
    }
  });
}

// ===== GCD helper for fraction reduction =====
function gcdForFraction(a, b) {
  a = Math.abs(Math.round(a));
  b = Math.abs(Math.round(b));
  while (b) { let t = b; b = a % b; a = t; }
  return a;
}

// Convert decimal result to fraction string (e.g. 1.5 -> "3/2")
function toFraction(value) {
  if (!isFinite(value)) return value.toString();
  if (Number.isInteger(value)) return value.toString();
  // Use up to 10^10 precision
  const precision = 1e10;
  let num = Math.round(value * precision);
  let den = precision;
  const g = gcdForFraction(num, den);
  num = num / g;
  den = den / g;
  // If denominator is very large, just show decimal
  if (Math.abs(den) > 1e9) return value.toString();
  return (num < 0 && den > 0 ? '' : '') + num + '/' + den;
}

// Convert to scientific notation string (e.g. 12300 -> "1.23e4")
function toScientific(value) {
  if (!isFinite(value)) return value.toString();
  if (value === 0) return '0';
  const exp = Math.floor(Math.log10(Math.abs(value)));
  // Only show in scientific if abs(exp) >= 4 or abs(exp) <= -4
  if (Math.abs(exp) < 4) return value.toString();
  const mantissa = value / Math.pow(10, exp);
  const mantissaStr = parseFloat(mantissa.toPrecision(10)).toString();
  return mantissaStr + 'e' + exp;
}

function formatResultWithMode(value) {
  let base = formatResult(value);
  if (answerDisplayMode === 'decimal') return base;
  if (answerDisplayMode === 'fraction') {
    if (!isFinite(value) || Number.isInteger(value)) return base;
    return toFraction(value);
  }
  if (answerDisplayMode === 'scientific') {
    return toScientific(value);
  }
  return base;
}

function setAngleUnit(unit) {
  angleUnit = unit;
  document.getElementById('btnDEG').classList.toggle('active', unit === 'DEG');
  document.getElementById('btnRAD').classList.toggle('active', unit === 'RAD');
  [1, 2].forEach(id => {
    const s = state[id];
    if (s.isResultDisplayed && s.calcInput !== '') {
    }
  });
}
function setThousandsSep(enabled) {
  thousandsSep = enabled;
  [1, 2].forEach(id => {
    const s = state[id];
    if (s.isResultDisplayed && s.calcInput !== '') {
      const num = parseFloat(s.calcInput);
      if (!isNaN(num)) {
        const formatted = formatResultWithMode(num);
        s.displayInput = formatted;
        document.getElementById('display' + id).value = formatted;
      }
    }
  });
}
function formatResult(value) {
  if (!thousandsSep) return value.toString();
  const num = parseFloat(value);
  if (isNaN(num)) return value.toString();
  const parts = num.toString().split('.');
  parts[0] = parts[0].replace(/\B(?=(\d{3})+(?!\d))/g, ',');
  return parts.join('.');
}
let state = {
  1: { displayInput: '', calcInput: '', isResultDisplayed: false, isScientific: false, isExtraVisible: false },
  2: { displayInput: '', calcInput: '', isResultDisplayed: false, isScientific: false, isExtraVisible: false }
};
let isSplitMode = false;
let currentMode = 'normal';
let settingsPanelOpen = false;
// ===== Memo Panel State =====
let isMemoOpen = false;

function isMobile() {
  return window.innerWidth <= 600;
}

function toggleMemoPanel() {
  if (isMobile()) {
    // On mobile: show overlay panel
    const overlay = document.getElementById('memoOverlayPanel');
    const isActive = overlay.classList.toggle('active');
    if (isActive) {
      document.getElementById('settingsOverlayPanel').classList.remove('active');
      document.getElementById('appsMenu').classList.remove('active');
    }
  } else {
    // On desktop: split-style, show memo panel to right of calculator
    isMemoOpen = !isMemoOpen;
    const memoPanel = document.getElementById('memoPanel');
    const container = document.getElementById('calcContainer');
    if (isMemoOpen) {
      memoPanel.classList.add('active');
      container.style.gap = '40px';
      // Sync textareas
      syncMemoTextareas('panel');
      document.getElementById('settingsOverlayPanel').classList.remove('active');
      document.getElementById('appsMenu').classList.remove('active');
    } else {
      memoPanel.classList.remove('active');
      if (!isSplitMode) container.style.gap = '0';
    }
  }
}

function syncMemoTextareas(source) {
  const panelTA = document.getElementById('memoPanelTextarea');
  const overlayTA = document.getElementById('memoOverlayTextarea');
  if (source === 'panel') {
    overlayTA.value = panelTA.value;
  } else {
    panelTA.value = overlayTA.value;
  }
}

// Keep memo textareas in sync
document.addEventListener('input', (e) => {
  if (e.target.id === 'memoPanelTextarea') syncMemoTextareas('panel');
  if (e.target.id === 'memoOverlayTextarea') syncMemoTextareas('overlay');
});

function clearMemo() {
  document.getElementById('memoPanelTextarea').value = '';
  document.getElementById('memoOverlayTextarea').value = '';
}

function toggleSettingsPanel() {
  const settingsOverlayPanel = document.getElementById('settingsOverlayPanel');
  const appsMenu = document.getElementById('appsMenu');
  // Close apps menu if open
  appsMenu.classList.remove('active');
  settingsOverlayPanel.classList.toggle('active');
}
function toggleSplitMode() {
  const container = document.getElementById('calcContainer');
  const calc2 = document.getElementById('calc2');
  const inputCalcPanel2 = document.getElementById('inputCalcPanel2');
  const icon = document.getElementById('splitIcon');
  isSplitMode = !isSplitMode;
  if (isSplitMode) {
    container.style.gap = "40px";
    icon.innerText = 'fullscreen';
    if (currentMode === 'inputcalc') {
      inputCalcPanel2.classList.add('active');
    } else {
      calc2.classList.remove('hidden');
    }
  } else {
    container.style.gap = isMemoOpen ? "40px" : "0";
    icon.innerText = 'view_agenda';
    calc2.classList.add('hidden');
    inputCalcPanel2.classList.remove('active');
  }
}
function setMode(id, type) {
  const menuNormal = document.getElementById('menuNormal');
  const menuScientific = document.getElementById('menuScientific');
  const menuInputCalc = document.getElementById('menuInputCalc');
  const menuMathTools = document.getElementById('menuMathTools');
  const inputCalcPanel = document.getElementById('inputCalcPanel');
  const inputCalcPanel2 = document.getElementById('inputCalcPanel2');
  const mathToolsPanel = document.getElementById('mathToolsPanel');
  const calc1 = document.getElementById('calc1');
  const calc2 = document.getElementById('calc2');
  menuNormal.classList.remove('selected');
  menuScientific.classList.remove('selected');
  menuInputCalc.classList.remove('selected');
  menuMathTools.classList.remove('selected');
  if (type === 'mathtools') {
    calc1.classList.add('hidden');
    if (isSplitMode) {
      calc2.classList.add('hidden');
      inputCalcPanel2.classList.remove('active');
    }
    inputCalcPanel.classList.remove('active');
    mathToolsPanel.classList.add('active');
    menuMathTools.classList.add('selected');
    currentMode = 'mathtools';
    return;
  }
  if (type === 'inputcalc') {
    calc1.classList.add('hidden');
    mathToolsPanel.classList.remove('active');
    inputCalcPanel.classList.add('active');
    menuInputCalc.classList.add('selected');
    currentMode = 'inputcalc';
    if (isSplitMode) {
      calc2.classList.add('hidden');
      inputCalcPanel2.classList.add('active');
    }
    setTimeout(() => {
      document.getElementById('inputCalcField').focus();
    }, 300);
    return;
  }
  mathToolsPanel.classList.remove('active');
  inputCalcPanel.classList.remove('active');
  inputCalcPanel2.classList.remove('active');
  currentMode = type;
  if (type === 'scientific') {
    menuScientific.classList.add('selected');
  } else {
    menuNormal.classList.add('selected');
  }
  calc1.classList.remove('hidden');
  [1, 2].forEach(calcId => {
    const calc = document.getElementById('calc' + calcId);
    state[calcId].isExtraVisible = false;
    if (type === 'scientific') {
      state[calcId].isScientific = true;
      calc.classList.add('scientific');
      renderScientificButtons(calcId);
    } else {
      state[calcId].isScientific = false;
      calc.classList.remove('scientific');
      renderNormalButtons(calcId);
    }
  });
  if (isSplitMode) {
    calc2.classList.remove('hidden');
  } else {
    calc2.classList.add('hidden');
  }
}
// ===== 上付き文字変換 =====
const superscriptMap = {
  '0': '⁰', '1': '¹', '2': '²', '3': '³', '4': '⁴',
  '5': '⁵', '6': '⁶', '7': '⁷', '8': '⁸', '9': '⁹',
  '-': '⁻', '+': '⁺'
};
function toSuperscript(str) {
  return str.split('').map(c => superscriptMap[c] || c).join('');
}
// ===== 普通計算用: displayInputの表示整形 =====
function formatDisplayWithSuperscript(displayStr) {
  return displayStr.replace(/\^(-?\d+\.?\d*)/g, (match, exp) => {
    return toSuperscript(exp);
  });
}
function renderNormalButtons(id) {
  const grid = document.getElementById('grid' + id);
  grid.innerHTML = `
    <button class="calc-btn" onclick="appendNumber('7', ${id})">7</button>
    <button class="calc-btn" onclick="appendNumber('8', ${id})">8</button>
    <button class="calc-btn" id="btn3_${id}" onclick="appendNumber('9', ${id})">9</button>
    <button class="calc-btn orange" onclick="appendOperator('÷','/', ${id})">÷</button>
    <button class="calc-btn" onclick="appendNumber('4', ${id})">4</button>
    <button class="calc-btn" onclick="appendNumber('5', ${id})">5</button>
    <button class="calc-btn" onclick="appendNumber('6', ${id})">6</button>
    <button class="calc-btn orange" onclick="appendOperator('×','*', ${id})">×</button>
    <button class="calc-btn" onclick="appendNumber('1', ${id})">1</button>
    <button class="calc-btn" id="btn2_${id}" onclick="appendNumber('2', ${id})">2</button>
    <button class="calc-btn" id="btn3num_${id}" onclick="appendNumber('3', ${id})">3</button>
    <button class="calc-btn orange" onclick="appendOperator('-','-', ${id})">−</button>
    <button class="calc-btn" onclick="appendNumber('0', ${id})">0</button>
    <button class="calc-btn" id="dotBtn${id}" onclick="appendNumber('.', ${id})">.</button>
    <button class="calc-btn" id="caretBtn${id}" onclick="appendCaretOperator(${id})">x²</button>
    <button class="calc-btn orange" onclick="appendOperator('+','+', ${id})">+</button>
    <button class="calc-btn red" onclick="clearDisplay(${id})">C</button>
    <button class="calc-btn red" onclick="backspace(${id})">⌫</button>
    <button class="calc-btn wide green" onclick="calculate(${id})">=</button>
  `;
  setupDotButtonFlick(id);
  setupCaretButtonFlick(id);
  setup3ButtonFlick(id);
  setup2ButtonFlick(id);
}
function toggleExtraButtons(id) {
  state[id].isExtraVisible = !state[id].isExtraVisible;
  renderScientificButtons(id);
}
function renderScientificButtons(id) {
  const grid = document.getElementById('grid' + id);
  const isExtra = state[id].isExtraVisible;
  
  if (!isExtra) {
    grid.innerHTML = `
      <button class="calc-btn blue" onclick="appendOperator('sin(','Math.sin(', ${id})">sin</button>
      <button class="calc-btn blue" onclick="appendOperator('cos(','Math.cos(', ${id})">cos</button>
      <button class="calc-btn blue" onclick="appendOperator('tan(','Math.tan(', ${id})">tan</button>
      <button class="calc-btn blue" onclick="appendOperator('√(', 'Math.sqrt(', ${id})">√</button>
      <button class="calc-btn blue" onclick="appendOperator('π', 'Math.PI', ${id})">π</button>
      <button class="calc-btn" onclick="appendNumber('7', ${id})">7</button>
      <button class="calc-btn" onclick="appendNumber('8', ${id})">8</button>
      <button class="calc-btn" onclick="appendNumber('9', ${id})">9</button>
      <button class="calc-btn blue" onclick="appendNumber('(', ${id})">(</button>
      <button class="calc-btn blue" onclick="appendNumber(')', ${id})">)</button>
      <button class="calc-btn" onclick="appendNumber('4', ${id})">4</button>
      <button class="calc-btn" onclick="appendNumber('5', ${id})">5</button>
      <button class="calc-btn" onclick="appendNumber('6', ${id})">6</button>
      <button class="calc-btn orange" onclick="appendOperator('÷','/', ${id})">÷</button>
      <button class="calc-btn orange" onclick="appendOperator('×','*', ${id})">×</button>
      <button class="calc-btn" onclick="appendNumber('1', ${id})">1</button>
      <button class="calc-btn" onclick="appendNumber('2', ${id})">2</button>
      <button class="calc-btn" onclick="appendNumber('3', ${id})">3</button>
      <button class="calc-btn orange" onclick="appendOperator('-','-', ${id})">−</button>
      <button class="calc-btn orange" onclick="appendOperator('+','+', ${id})">+</button>
      <button class="calc-btn" onclick="appendNumber('0', ${id})">0</button>
      <button class="calc-btn" onclick="appendNumber('.', ${id})">.</button>
      <button class="calc-btn blue" onclick="appendNumber('-', ${id})">(-)</button>
      <button class="calc-btn red" onclick="clearDisplay(${id})">C</button>
      <button class="calc-btn red" onclick="backspace(${id})">⌫</button>
      <button class="calc-btn blue" onclick="appendOperator('/','/', ${id})">/</button>
      <button class="calc-btn blue" onclick="appendOperatorScientificCaret(${id})">xⁿ</button>
      <button class="calc-btn blue" onclick="toggleExtraButtons(${id})">···</button>
      <button class="calc-btn wide green" onclick="calculate(${id})">=</button>
    `;
  } else {
    grid.innerHTML = `
      <button class="calc-btn blue" onclick="appendOperator('asin(','Math.asin(', ${id})">sin⁻¹</button>
      <button class="calc-btn blue" onclick="appendOperator('acos(','Math.acos(', ${id})">cos⁻¹</button>
      <button class="calc-btn blue" onclick="appendOperator('atan(','Math.atan(', ${id})">tan⁻¹</button>
      <button class="calc-btn blue" onclick="appendOperator('log(','Math.log10(', ${id})">log</button>
      <button class="calc-btn blue" onclick="appendOperator('abs(','Math.abs(', ${id})">abs</button>
      <button class="calc-btn" onclick="appendNumber('7', ${id})">7</button>
      <button class="calc-btn" onclick="appendNumber('8', ${id})">8</button>
      <button class="calc-btn" onclick="appendNumber('9', ${id})">9</button>
      <button class="calc-btn blue" onclick="appendOperator('e','Math.E', ${id})">e</button>
      <button class="calc-btn blue" onclick="appendOperator('!','!', ${id})">!</button>
      <button class="calc-btn" onclick="appendNumber('4', ${id})">4</button>
      <button class="calc-btn" onclick="appendNumber('5', ${id})">5</button>
      <button class="calc-btn" onclick="appendNumber('6', ${id})">6</button>
      <button class="calc-btn orange" onclick="appendOperator('÷','/', ${id})">÷</button>
      <button class="calc-btn orange" onclick="appendOperator('×','*', ${id})">×</button>
      <button class="calc-btn" onclick="appendNumber('1', ${id})">1</button>
      <button class="calc-btn" onclick="appendNumber('2', ${id})">2</button>
      <button class="calc-btn" onclick="appendNumber('3', ${id})">3</button>
      <button class="calc-btn orange" onclick="appendOperator('-','-', ${id})">−</button>
      <button class="calc-btn orange" onclick="appendOperator('+','+', ${id})">+</button>
      <button class="calc-btn blue" onclick="appendNumber('0', ${id})">0</button>
      <button class="calc-btn blue" onclick="appendNumber('.', ${id})">.</button>
      <button class="calc-btn blue" onclick="appendNumber('-', ${id})">(-)</button>
      <button class="calc-btn red" onclick="clearDisplay(${id})">C</button>
      <button class="calc-btn red" onclick="backspace(${id})">⌫</button>
      <button class="calc-btn blue" onclick="appendOperator('P','P', ${id})">P</button>
      <button class="calc-btn blue" onclick="appendOperator('C','C', ${id})">C</button>
      <button class="calc-btn blue" onclick="toggleExtraButtons(${id})">···</button>
      <button class="calc-btn wide green" onclick="calculate(${id})">=</button>
    `;
  }
}
// ===== 関数計算用: xⁿボタン（^をそのまま表示） =====
function appendOperatorScientificCaret(id) {
  let s = state[id];
  s.isResultDisplayed = false;
  if (s.calcInput === '') return;
  s.calcInput += '^';
  s.displayInput += '^';
  updateDisplay(id);
}
// ===== 普通計算用: x²ボタン（^を入力、数字が来たら上付き変換） =====
function appendCaretOperator(id) {
  let s = state[id];
  s.isResultDisplayed = false;
  if (s.calcInput === '') return;
  s.calcInput += '^';
  s.displayInput += '^';
  updateDisplay(id);
}
// ===== 負の数判定つきappendNumber（普通計算用） =====
function appendNumber(num, id) {
  let s = state[id];
  if (s.isResultDisplayed) {
    s.displayInput = '';
    s.calcInput = '';
    s.isResultDisplayed = false;
  }
  if (!s.isScientific && num === '-') {
    s.displayInput += num;
    s.calcInput += num;
    updateDisplay(id);
    return;
  }
  s.displayInput += num;
  s.calcInput += num;
  updateDisplay(id);
}
// ===== 普通計算の−ボタン: 負の数対応 =====
function appendOperator(displayOp, calcOp, id) {
  let s = state[id];
  s.isResultDisplayed = false;
  const noInputTriggers = ['Math.PI', 'Math.E', 'Math.sin(', 'Math.cos(', 'Math.tan(', 'Math.sqrt(', 'Math.log10(', 'Math.log(', 'Math.abs(', 'Math.asin(', 'Math.acos(', 'Math.atan('];
  
  if (!s.isScientific && calcOp === '-') {
    const calc = s.calcInput;
    if (calc === '') {
      s.displayInput += '-';
      s.calcInput += '-';
      updateDisplay(id);
      return;
    }
    const lastChar = calc.slice(-1);
    if (['+', '-', '*', '/', '^', '('].includes(lastChar)) {
      if (lastChar === '-') return;
      s.displayInput += '-';
      s.calcInput += '-';
      updateDisplay(id);
      return;
    }
  }
  if(s.calcInput === '' && !noInputTriggers.includes(calcOp)) return;
  
  const lastChar = s.calcInput.slice(-1);
  if(['+', '-', '*', '/', '^2', '**'].includes(lastChar) && !calcOp.includes('Math')) {
    s.displayInput = s.displayInput.slice(0, -1);
    s.calcInput = s.calcInput.slice(0, -1);
  }
  s.displayInput += displayOp;
  s.calcInput += calcOp;
  updateDisplay(id);
}

// ===== リアルタイム式プレビュー =====
function getRealtimePreview(calcInput) {
  if (!calcInput || calcInput === '') return '';
  try {
    // Try to evaluate partially - build a safe preview string
    let expr = calcInput;
    // Close unclosed brackets
    let open = (expr.match(/\(/g) || []).length;
    let close = (expr.match(/\)/g) || []).length;
    for (let i = 0; i < open - close; i++) expr += ')';
    // Remove trailing operators
    expr = expr.replace(/[\+\-\*\/\^]+$/, '');
    if (!expr) return '';
    let evalExpr = expr.replace(/\^(-?\d+\.?\d*)/g, '**($1)').replace(/\^/g, '**');
    evalExpr = evalExpr.replace(/(\d+)!/g, "factorial($1)");
    evalExpr = evalExpr.replace(/(\d+)P(\d+)/g, "nPr($1,$2)");
    evalExpr = evalExpr.replace(/(\d+)C(\d+)/g, "nCr($1,$2)");
    if (angleUnit === 'DEG') {
      evalExpr = evalExpr.replace(/Math\.(sin|cos|tan)\(/g, 'Math.$1(Math.PI/180*');
      evalExpr = evalExpr.replace(/Math\.(asin|acos|atan)\(/g, '(180/Math.PI)*Math.$1(');
    }
    let result = eval(evalExpr);
    if (result === undefined || result === null) return '';
    result = Math.round(result * 10000000000) / 10000000000;
    if (!isFinite(result)) return '';
    return '= ' + formatResultWithMode(result);
  } catch {
    return '';
  }
}

// ===== updateDisplay: 普通計算と関数計算で表示整形を分ける =====
function updateDisplay(id) {
  let s = state[id];
  const displayElement = document.getElementById('display' + id);
  let displayValue;
  if (s.isScientific) {
    displayValue = s.displayInput;
  } else {
    displayValue = formatDisplayWithSuperscript(s.displayInput);
  }
  displayElement.value = displayValue;
  const isOverflow = displayElement.scrollWidth > displayElement.clientWidth;
  if (isOverflow) {
    displayElement.style.textAlign = 'left';
    displayElement.scrollLeft = 0;
  } else {
    displayElement.style.textAlign = 'right';
    displayElement.scrollLeft = 0;
  }
  // Update realtime expression preview
  const realtimeEl = document.getElementById('realtimeExpr' + id);
  if (realtimeEl) {
    if (!s.isResultDisplayed && s.calcInput !== '') {
      realtimeEl.textContent = getRealtimePreview(s.calcInput);
    } else {
      realtimeEl.textContent = '';
    }
  }
}
// 階乗計算用
function factorial(n) {
  if (n < 0) return NaN;
  if (n === 0) return 1;
  let res = 1;
  for (let i = 2; i <= n; i++) res *= i;
  return res;
}
// コンビネーション・パーミュテーション用
function nCr(n, r) {
  if (r < 0 || r > n) return 0;
  return factorial(n) / (factorial(r) * factorial(n - r));
}
function nPr(n, r) {
  if (r < 0 || r > n) return 0;
  return factorial(n) / factorial(n - r);
}
function calculate(id) {
  let s = state[id];
  if (s.calcInput === '') return;
  try {
    let historyDisplay;
    if (s.isScientific) {
      historyDisplay = s.displayInput;
    } else {
      historyDisplay = formatDisplayWithSuperscript(s.displayInput);
    }
    document.getElementById('history' + id).innerText = historyDisplay;
    let openBrackets = (s.calcInput.match(/\(/g) || []).length;
    let closeBrackets = (s.calcInput.match(/\)/g) || []).length;
    let finalExpr = s.calcInput;
    for(let i=0; i < openBrackets - closeBrackets; i++) {
      finalExpr += ')';
    }
    
    let expression = finalExpr.replace(/\^(-?\d+\.?\d*)/g, '**($1)');
    expression = expression.replace(/\^/g, '**');
    expression = expression.replace(/(\d+)!/g, "factorial($1)");
    expression = expression.replace(/(\d+)P(\d+)/g, "nPr($1,$2)");
    expression = expression.replace(/(\d+)C(\d+)/g, "nCr($1,$2)");
    if (angleUnit === 'DEG') {
      expression = expression.replace(/Math\.(sin|cos|tan)\(/g, 'Math.$1(Math.PI/180*');
      expression = expression.replace(/Math\.(asin|acos|atan)\(/g, '(180/Math.PI)*Math.$1(');
    }
    let result = eval(expression);
    result = Math.round(result * 10000000000) / 10000000000;
    const formatted = formatResultWithMode(result);
    s.displayInput = formatted;
    s.calcInput = result.toString();
    const displayEl = document.getElementById('display' + id);
    displayEl.value = formatted;
    const isOverflow = displayEl.scrollWidth > displayEl.clientWidth;
    if (isOverflow) {
      displayEl.style.textAlign = 'left';
      displayEl.scrollLeft = 0;
    } else {
      displayEl.style.textAlign = 'right';
      displayEl.scrollLeft = 0;
    }
    // Clear realtime preview after result
    const realtimeEl = document.getElementById('realtimeExpr' + id);
    if (realtimeEl) realtimeEl.textContent = '';
    s.isResultDisplayed = true;
  } catch {
    document.getElementById('display' + id).value = 'Error';
    s.displayInput = '';
    s.calcInput = '';
    s.isResultDisplayed = false;
  }
}
function clearDisplay(id) {
  let s = state[id];
  s.displayInput = '';
  s.calcInput = '';
  s.isResultDisplayed = false;
  const displayEl = document.getElementById('display' + id);
  displayEl.value = '';
  displayEl.style.textAlign = 'right';
  document.getElementById('history' + id).innerText = '';
  const realtimeEl = document.getElementById('realtimeExpr' + id);
  if (realtimeEl) realtimeEl.textContent = '';
}
function backspace(id) {
  let s = state[id];
  if (s.isResultDisplayed) return;
  
  const patterns = ['Math.sin(', 'Math.cos(', 'Math.tan(', 'Math.sqrt(', 'Math.log10(', 'Math.log(', 'Math.PI', 'Math.E'];
  let deleted = false;
  for(let p of patterns) {
    if (s.calcInput.endsWith(p)) {
      s.calcInput = s.calcInput.slice(0, -p.length);
      s.displayInput = s.displayInput.slice(0, -(p.includes('Math.') ? (p.length-5) : 1));
      deleted = true;
      break;
    }
  }
  
  if (!deleted) {
    s.displayInput = s.displayInput.slice(0, -1);
    s.calcInput = s.calcInput.slice(0, -1);
  }
  updateDisplay(id);
}
// ===== Input Calculator =====
const superToNormal = {
  '¹': '1', '²': '2', '³': '3', '⁴': '4', '⁵': '5',
  '⁶': '6', '⁷': '7', '⁸': '8', '⁹': '9', '⁰': '0',
  '⁺': '+', '⁻': '-', 'ˣ': '*', 'ᐟ': '/', '‧': '*',
  '⁽': '(', '⁾': ')'
};
function normalizeInputCalcExpr(raw) {
  let expr = raw;
  expr = expr.replace(/([¹²³⁴⁵⁶⁷⁸⁹⁰⁺⁻]+)/g, (match) => {
    const converted = match.split('').map(c => superToNormal[c] || c).join('');
    return '^(' + converted + ')';
  });
  expr = expr.replace(/ˣ/g, '*');
  expr = expr.replace(/ᐟ/g, '/');
  expr = expr.replace(/‧/g, '*');
  expr = expr.replace(/⁽/g, '(');
  expr = expr.replace(/⁾/g, ')');
  expr = expr.replace(/÷/g, '/');
  expr = expr.replace(/×/g, '*');
  expr = expr.replace(/\^/g, '**');
  expr = expr.replace(/√\(([^)]*)\)/g, 'Math.sqrt($1)');
  expr = expr.replace(/√(\d+\.?\d*)/g, 'Math.sqrt($1)');
  expr = expr.replace(/π/g, 'Math.PI');
  expr = expr.replace(/\bsin\b/g, 'Math.sin');
  expr = expr.replace(/\bcos\b/g, 'Math.cos');
  expr = expr.replace(/\btan\b/g, 'Math.tan');
  expr = expr.replace(/\bsqrt\b/g, 'Math.sqrt');
  expr = expr.replace(/\blog\b/g, 'Math.log10');
  expr = expr.replace(/\babs\b/g, 'Math.abs');
  expr = expr.replace(/\bPI\b/g, 'Math.PI');
  expr = expr.replace(/\bE\b/g, 'Math.E');
  expr = expr.replace(/(\d+)!/g, 'factorial($1)');
  if (angleUnit === 'DEG') {
    expr = expr.replace(/Math\.(sin|cos|tan)\(/g, 'Math.$1(Math.PI/180*');
    expr = expr.replace(/Math\.(asin|acos|atan)\(/g, '(180/Math.PI)*Math.$1(');
  }
  return expr;
}

// ===== Input Calculator Realtime Preview =====
function inputCalcRealtimeUpdate(panelNum) {
  const fieldId = panelNum === 1 ? 'inputCalcField' : 'inputCalcField2';
  const realtimeId = panelNum === 1 ? 'inputCalcRealtime' : 'inputCalcRealtime2';
  const field = document.getElementById(fieldId);
  const realtimeEl = document.getElementById(realtimeId);
  if (!field || !realtimeEl) return;
  const raw = field.value.trim();
  if (!raw) { realtimeEl.textContent = ''; return; }
  try {
    const expression = normalizeInputCalcExpr(raw);
    let result = eval(expression);
    if (result === undefined || result === null || !isFinite(result)) { realtimeEl.textContent = ''; return; }
    result = Math.round(result * 10000000000) / 10000000000;
    realtimeEl.textContent = '= ' + formatResultWithMode(result);
  } catch {
    realtimeEl.textContent = '';
  }
}

function inputCalcEvaluate() {
  const field = document.getElementById('inputCalcField');
  const exprEl = document.getElementById('inputCalcExpr');
  const resultEl = document.getElementById('inputCalcResult');
  const realtimeEl = document.getElementById('inputCalcRealtime');
  const raw = field.value.trim();
  if (!raw) return;
  exprEl.textContent = raw;
  if (realtimeEl) realtimeEl.textContent = '';
  try {
    const expression = normalizeInputCalcExpr(raw);
    let result = eval(expression);
    result = Math.round(result * 10000000000) / 10000000000;
    resultEl.textContent = formatResultWithMode(result);
    resultEl.scrollLeft = 0;
  } catch {
    resultEl.textContent = I18N[currentLang].inputCalcError || 'Error';
  }
}
function inputCalcEvaluate2() {
  const field = document.getElementById('inputCalcField2');
  const exprEl = document.getElementById('inputCalcExpr2');
  const resultEl = document.getElementById('inputCalcResult2');
  const realtimeEl = document.getElementById('inputCalcRealtime2');
  const raw = field.value.trim();
  if (!raw) return;
  exprEl.textContent = raw;
  if (realtimeEl) realtimeEl.textContent = '';
  try {
    const expression = normalizeInputCalcExpr(raw);
    let result = eval(expression);
    result = Math.round(result * 10000000000) / 10000000000;
    resultEl.textContent = formatResultWithMode(result);
    resultEl.scrollLeft = 0;
  } catch {
    resultEl.textContent = I18N[currentLang].inputCalcError || 'Error';
  }
}
// ===== Math Tools =====
function openMathTab(index) {
  document.querySelectorAll('.math-tab').forEach((t, i) => t.classList.toggle('active', i === index));
  document.querySelectorAll('.math-tab-content').forEach((c, i) => c.classList.toggle('active', i === index));
}
function addMathInput(containerId) {
  const container = document.getElementById(containerId);
  const div = document.createElement("div");
  div.className = "math-input-group";
  div.innerHTML = `
    <input type="number" min="1" placeholder="数字を入力" class="math-number-input">
    <button class="math-remove-btn" onclick="removeMathInput(this)">
      <span class="material-symbols-rounded">delete</span>
    </button>
  `;
  container.appendChild(div);
}
function removeMathInput(btn) {
  if (btn.parentElement.parentElement.children.length > 1) {
    btn.parentElement.remove();
  }
}
['gcdInputs','lcmInputs'].forEach(id => addMathInput(id));
function getMathValues(id) {
  return [...document.querySelectorAll(`#${id} input`)]
    .map(i => Number(i.value))
    .filter(v => v > 0);
}
function primeFactor() {
  let n = Number(document.getElementById('primeInput').value);
  const resultEl = document.getElementById('primeResult');
  if (!n || n < 2) { resultEl.classList.remove('visible'); return; }
  let res = [];
  let d = 2;
  let tempN = n;
  while(tempN > 1){
    while(tempN % d === 0){
      res.push(d);
      tempN /= d;
    }
    d++;
  }
  const t = I18N[currentLang];
  resultEl.innerHTML = "<strong>" + (t.mathResultPrime || 'Result') + ":</strong> " + res.join(" × ");
  resultEl.classList.add('visible');
}
function commonDivisors() {
  let nums = getMathValues("gcdInputs");
  const resultEl = document.getElementById('gcdResult');
  if (!nums.length) { resultEl.classList.remove('visible'); return; }
  let min = Math.min(...nums);
  let res = [];
  for (let i = 1; i <= min; i++){
    if(nums.every(n => n % i === 0)) res.push(i);
  }
  const t = I18N[currentLang];
  resultEl.innerHTML = "<strong>" + (t.mathResultGcd || 'GCD') + ":</strong> " + res.join(", ");
  resultEl.classList.add('visible');
}
function mathGcd(a, b) { return b ? mathGcd(b, a % b) : a; }
function mathLcm(a, b) { return (a * b) / mathGcd(a, b); }
function commonMultiples() {
  let nums = getMathValues("lcmInputs");
  const resultEl = document.getElementById('lcmResult');
  if (!nums.length) { resultEl.classList.remove('visible'); return; }
  let result = nums.reduce((a, b) => mathLcm(a, b));
  const t = I18N[currentLang];
  resultEl.innerHTML = "<strong>" + (t.mathResultLcm || 'LCM') + ":</strong> " + result;
  resultEl.classList.add('visible');
}
// ===== 汎用フリック状態管理 =====
function createFlickHandler(options) {
  let active = false;
  let startX = 0, startY = 0;
  let direction = null;
  let longPressTimer = null;
  let popupVisible = false;
  function showPopup(btn, dir) {
    const popup = document.getElementById(options.popupId);
    const rect = btn.getBoundingClientRect();
    popup.style.display = 'block';
    popup.style.left = (rect.left + rect.width / 2 - 80) + 'px';
    popup.style.top = (rect.top - 70) + 'px';
    updatePopup(dir);
  }
  function updatePopup(dir) {
    const leftEl = document.getElementById(options.leftId);
    const centerEl = document.getElementById(options.centerId);
    const rightEl = document.getElementById(options.rightId);
    leftEl.classList.toggle('active', dir === 'left');
    centerEl.classList.toggle('active', dir === null);
    rightEl.classList.toggle('active', dir === 'right');
  }
  function hidePopup() {
    const popup = document.getElementById(options.popupId);
    popup.style.display = 'none';
  }
  function setup(btn) {
    if (!btn) return;
    btn.addEventListener('touchstart', (e) => {
      const touch = e.touches[0];
      active = true;
      startX = touch.clientX;
      startY = touch.clientY;
      direction = null;
      popupVisible = false;
      longPressTimer = setTimeout(() => {
        showPopup(btn, null);
        popupVisible = true;
      }, 400);
      e.preventDefault();
    }, { passive: false });
    btn.addEventListener('touchmove', (e) => {
      if (!active) return;
      const touch = e.touches[0];
      const dx = touch.clientX - startX;
      const dy = touch.clientY - startY;
      if (Math.abs(dx) > 15 || Math.abs(dy) > 15) {
        clearTimeout(longPressTimer);
      }
      let dir = null;
      if (Math.abs(dx) > Math.abs(dy) + 10) {
        dir = dx < 0 ? 'left' : 'right';
      }
      direction = dir;
      if (popupVisible) updatePopup(dir);
      e.preventDefault();
    }, { passive: false });
    btn.addEventListener('touchend', (e) => {
      clearTimeout(longPressTimer);
      hidePopup();
      if (!active) return;
      active = false;
      if (direction === 'left') {
        options.onLeft();
      } else if (direction === 'right') {
        options.onRight();
      } else {
        options.onCenter();
      }
      direction = null;
      e.preventDefault();
    }, { passive: false });
    btn.addEventListener('mousedown', (e) => {
      active = true;
      startX = e.clientX;
      startY = e.clientY;
      direction = null;
      popupVisible = false;
      longPressTimer = setTimeout(() => {
        showPopup(btn, null);
        popupVisible = true;
      }, 400);
      btn._flickActive = true;
      btn._flickStartX = startX;
      btn._flickStartY = startY;
      btn._flickDir = null;
      btn._flickPopupVisible = false;
      btn._flickShowPopup = showPopup;
      btn._flickUpdatePopup = updatePopup;
      btn._flickHidePopup = hidePopup;
      btn._flickOnLeft = options.onLeft;
      btn._flickOnCenter = options.onCenter;
      btn._flickOnRight = options.onRight;
    });
  }
  return { setup };
}
// ===== .ボタン フリック入力（普通計算用） =====
let flickState = {
  active: false,
  startX: 0,
  startY: 0,
  direction: null,
  calcId: null,
  longPressTimer: null,
  popupVisible: false,
  btnType: null
};
function setupDotButtonFlick(id) {
  const btn = document.getElementById('dotBtn' + id);
  if (!btn) return;
  btn.addEventListener('touchstart', (e) => {
    const touch = e.touches[0];
    flickState.active = true;
    flickState.startX = touch.clientX;
    flickState.startY = touch.clientY;
    flickState.direction = null;
    flickState.calcId = id;
    flickState.popupVisible = false;
    flickState.btnType = 'dot';
    flickState.longPressTimer = setTimeout(() => {
      showFlickPopup(btn, null, 'dot');
      flickState.popupVisible = true;
    }, 400);
    e.preventDefault();
  }, { passive: false });
  btn.addEventListener('touchmove', (e) => {
    if (!flickState.active || flickState.btnType !== 'dot') return;
    const touch = e.touches[0];
    const dx = touch.clientX - flickState.startX;
    const dy = touch.clientY - flickState.startY;
    if (Math.abs(dx) > 15 || Math.abs(dy) > 15) {
      clearTimeout(flickState.longPressTimer);
    }
    let dir = null;
    if (Math.abs(dx) > Math.abs(dy) + 10) {
      dir = dx < 0 ? 'left' : 'right';
    }
    flickState.direction = dir;
    if (flickState.popupVisible) updateFlickPopup(dir, 'dot');
    e.preventDefault();
  }, { passive: false });
  btn.addEventListener('touchend', (e) => {
    if (flickState.btnType !== 'dot') return;
    clearTimeout(flickState.longPressTimer);
    hideFlickPopup('dot');
    if (!flickState.active) return;
    flickState.active = false;
    const cid = flickState.calcId;
    if (flickState.direction === 'left') {
      appendFlickChar('(', cid);
    } else if (flickState.direction === 'right') {
      appendFlickChar(')', cid);
    } else {
      appendNumber('.', cid);
    }
    flickState.direction = null;
    flickState.calcId = null;
    flickState.btnType = null;
    e.preventDefault();
  }, { passive: false });
  btn.addEventListener('mousedown', (e) => {
    flickState.active = true;
    flickState.startX = e.clientX;
    flickState.startY = e.clientY;
    flickState.direction = null;
    flickState.calcId = id;
    flickState.popupVisible = false;
    flickState.btnType = 'dot';
    flickState.longPressTimer = setTimeout(() => {
      showFlickPopup(btn, null, 'dot');
      flickState.popupVisible = true;
    }, 400);
  });
}
// ===== x²ボタン フリック入力（普通計算用） =====
function setupCaretButtonFlick(id) {
  const btn = document.getElementById('caretBtn' + id);
  if (!btn) return;
  btn.addEventListener('touchstart', (e) => {
    const touch = e.touches[0];
    flickState.active = true;
    flickState.startX = touch.clientX;
    flickState.startY = touch.clientY;
    flickState.direction = null;
    flickState.calcId = id;
    flickState.popupVisible = false;
    flickState.btnType = 'caret';
    flickState.longPressTimer = setTimeout(() => {
      showFlickPopup(btn, null, 'caret');
      flickState.popupVisible = true;
    }, 400);
    e.preventDefault();
  }, { passive: false });
  btn.addEventListener('touchmove', (e) => {
    if (!flickState.active || flickState.btnType !== 'caret') return;
    const touch = e.touches[0];
    const dx = touch.clientX - flickState.startX;
    const dy = touch.clientY - flickState.startY;
    if (Math.abs(dx) > 15 || Math.abs(dy) > 15) {
      clearTimeout(flickState.longPressTimer);
    }
    let dir = null;
    if (Math.abs(dx) > Math.abs(dy) + 10) {
      dir = dx < 0 ? 'left' : 'right';
    }
    flickState.direction = dir;
    if (flickState.popupVisible) updateFlickPopup(dir, 'caret');
    e.preventDefault();
  }, { passive: false });
  btn.addEventListener('touchend', (e) => {
    if (flickState.btnType !== 'caret') return;
    clearTimeout(flickState.longPressTimer);
    hideFlickPopup('caret');
    if (!flickState.active) return;
    flickState.active = false;
    const cid = flickState.calcId;
    if (flickState.direction === 'left') {
      appendFlickChar('/', cid);
    } else if (flickState.direction === 'right') {
      appendFlickChar('!', cid);
    } else {
      appendCaretOperator(cid);
    }
    flickState.direction = null;
    flickState.calcId = null;
    flickState.btnType = null;
    e.preventDefault();
  }, { passive: false });
  btn.addEventListener('mousedown', (e) => {
    flickState.active = true;
    flickState.startX = e.clientX;
    flickState.startY = e.clientY;
    flickState.direction = null;
    flickState.calcId = id;
    flickState.popupVisible = false;
    flickState.btnType = 'caret';
    flickState.longPressTimer = setTimeout(() => {
      showFlickPopup(btn, null, 'caret');
      flickState.popupVisible = true;
    }, 400);
  });
}
// ===== 3ボタン フリック入力（普通計算用） =====
function setup3ButtonFlick(id) {
  const btn = document.getElementById('btn3num_' + id);
  if (!btn) return;
  btn.addEventListener('touchstart', (e) => {
    const touch = e.touches[0];
    flickState.active = true;
    flickState.startX = touch.clientX;
    flickState.startY = touch.clientY;
    flickState.direction = null;
    flickState.calcId = id;
    flickState.popupVisible = false;
    flickState.btnType = 'btn3';
    flickState.longPressTimer = setTimeout(() => {
      showFlickPopup(btn, null, 'btn3');
      flickState.popupVisible = true;
    }, 400);
    e.preventDefault();
  }, { passive: false });
  btn.addEventListener('touchmove', (e) => {
    if (!flickState.active || flickState.btnType !== 'btn3') return;
    const touch = e.touches[0];
    const dx = touch.clientX - flickState.startX;
    const dy = touch.clientY - flickState.startY;
    if (Math.abs(dx) > 15 || Math.abs(dy) > 15) {
      clearTimeout(flickState.longPressTimer);
    }
    let dir = null;
    if (Math.abs(dx) > Math.abs(dy) + 10) {
      dir = dx < 0 ? 'left' : 'right';
    }
    flickState.direction = dir;
    if (flickState.popupVisible) updateFlickPopup(dir, 'btn3');
    e.preventDefault();
  }, { passive: false });
  btn.addEventListener('touchend', (e) => {
    if (flickState.btnType !== 'btn3') return;
    clearTimeout(flickState.longPressTimer);
    hideFlickPopup('btn3');
    if (!flickState.active) return;
    flickState.active = false;
    const cid = flickState.calcId;
    if (flickState.direction === 'left') {
      appendFlickSqrt(cid);
    } else if (flickState.direction === 'right') {
      appendFlickPi(cid);
    } else {
      appendNumber('3', cid);
    }
    flickState.direction = null;
    flickState.calcId = null;
    flickState.btnType = null;
    e.preventDefault();
  }, { passive: false });
  btn.addEventListener('mousedown', (e) => {
    flickState.active = true;
    flickState.startX = e.clientX;
    flickState.startY = e.clientY;
    flickState.direction = null;
    flickState.calcId = id;
    flickState.popupVisible = false;
    flickState.btnType = 'btn3';
    flickState.longPressTimer = setTimeout(() => {
      showFlickPopup(btn, null, 'btn3');
      flickState.popupVisible = true;
    }, 400);
  });
}
// ===== 2ボタン フリック入力（普通計算用） =====
function setup2ButtonFlick(id) {
  const btn = document.getElementById('btn2_' + id);
  if (!btn) return;
  btn.addEventListener('touchstart', (e) => {
    const touch = e.touches[0];
    flickState.active = true;
    flickState.startX = touch.clientX;
    flickState.startY = touch.clientY;
    flickState.direction = null;
    flickState.calcId = id;
    flickState.popupVisible = false;
    flickState.btnType = 'btn2';
    flickState.longPressTimer = setTimeout(() => {
      showFlickPopup(btn, null, 'btn2');
      flickState.popupVisible = true;
    }, 400);
    e.preventDefault();
  }, { passive: false });
  btn.addEventListener('touchmove', (e) => {
    if (!flickState.active || flickState.btnType !== 'btn2') return;
    const touch = e.touches[0];
    const dx = touch.clientX - flickState.startX;
    const dy = touch.clientY - flickState.startY;
    if (Math.abs(dx) > 15 || Math.abs(dy) > 15) {
      clearTimeout(flickState.longPressTimer);
    }
    let dir = null;
    if (Math.abs(dx) > Math.abs(dy) + 10) {
      dir = dx < 0 ? 'left' : 'right';
    }
    flickState.direction = dir;
    if (flickState.popupVisible) updateFlickPopup(dir, 'btn2');
    e.preventDefault();
  }, { passive: false });
  btn.addEventListener('touchend', (e) => {
    if (flickState.btnType !== 'btn2') return;
    clearTimeout(flickState.longPressTimer);
    hideFlickPopup('btn2');
    if (!flickState.active) return;
    flickState.active = false;
    const cid = flickState.calcId;
    if (flickState.direction === 'left') {
      appendFlickChar('P', cid);
    } else if (flickState.direction === 'right') {
      appendFlickChar('C', cid);
    } else {
      appendNumber('2', cid);
    }
    flickState.direction = null;
    flickState.calcId = null;
    flickState.btnType = null;
    e.preventDefault();
  }, { passive: false });
  btn.addEventListener('mousedown', (e) => {
    flickState.active = true;
    flickState.startX = e.clientX;
    flickState.startY = e.clientY;
    flickState.direction = null;
    flickState.calcId = id;
    flickState.popupVisible = false;
    flickState.btnType = 'btn2';
    flickState.longPressTimer = setTimeout(() => {
      showFlickPopup(btn, null, 'btn2');
      flickState.popupVisible = true;
    }, 400);
  });
}
function appendFlickChar(char, id) {
  let s = state[id];
  if (s.isResultDisplayed) {
    s.displayInput = '';
    s.calcInput = '';
    s.isResultDisplayed = false;
  }
  s.displayInput += char;
  s.calcInput += char;
  updateDisplay(id);
}
function appendFlickSqrt(id) {
  let s = state[id];
  if (s.isResultDisplayed) {
    s.displayInput = '';
    s.calcInput = '';
    s.isResultDisplayed = false;
  }
  s.displayInput += '√(';
  s.calcInput += 'Math.sqrt(';
  updateDisplay(id);
}
function appendFlickPi(id) {
  let s = state[id];
  if (s.isResultDisplayed) {
    s.displayInput = '';
    s.calcInput = '';
    s.isResultDisplayed = false;
  }
  s.displayInput += 'π';
  s.calcInput += 'Math.PI';
  updateDisplay(id);
}
// ===== フリックポップアップ表示・更新・非表示（ボタン種別対応） =====
function showFlickPopup(btn, dir, btnType) {
  const popupId = {
    'dot': 'flickPopup',
    'caret': 'flickPopupCaret',
    'btn3': 'flickPopup3',
    'btn2': 'flickPopup2'
  }[btnType];
  if (!popupId) return;
  const popup = document.getElementById(popupId);
  const rect = btn.getBoundingClientRect();
  popup.style.display = 'block';
  popup.style.left = (rect.left + rect.width / 2 - 80) + 'px';
  popup.style.top = (rect.top - 70) + 'px';
  updateFlickPopup(dir, btnType);
}
function updateFlickPopup(dir, btnType) {
  const ids = {
    'dot':   { left: 'flickLeft',     center: 'flickCenter',     right: 'flickRight' },
    'caret': { left: 'flickCaretLeft', center: 'flickCaretCenter', right: 'flickCaretRight' },
    'btn3':  { left: 'flick3Left',    center: 'flick3Center',    right: 'flick3Right' },
    'btn2':  { left: 'flick2Left',    center: 'flick2Center',    right: 'flick2Right' }
  }[btnType];
  if (!ids) return;
  document.getElementById(ids.left).classList.toggle('active', dir === 'left');
  document.getElementById(ids.center).classList.toggle('active', dir === null);
  document.getElementById(ids.right).classList.toggle('active', dir === 'right');
}
function hideFlickPopup(btnType) {
  const popupId = {
    'dot': 'flickPopup',
    'caret': 'flickPopupCaret',
    'btn3': 'flickPopup3',
    'btn2': 'flickPopup2'
  }[btnType];
  if (!popupId) return;
  document.getElementById(popupId).style.display = 'none';
}
// ===== マウスイベント（PC用） =====
document.addEventListener('mouseup', (e) => {
  if (!flickState.active) return;
  clearTimeout(flickState.longPressTimer);
  const bt = flickState.btnType;
  hideFlickPopup(bt);
  flickState.active = false;
  const cid = flickState.calcId;
  if (cid === null) { flickState.btnType = null; return; }
  const dx = e.clientX - flickState.startX;
  let dir = null;
  if (Math.abs(dx) > 20) dir = dx < 0 ? 'left' : 'right';
  if (bt === 'dot') {
    if (dir === 'left') appendFlickChar('(', cid);
    else if (dir === 'right') appendFlickChar(')', cid);
  } else if (bt === 'caret') {
    if (dir === 'left') appendFlickChar('/', cid);
    else if (dir === 'right') appendFlickChar('!', cid);
  } else if (bt === 'btn3') {
    if (dir === 'left') appendFlickSqrt(cid);
    else if (dir === 'right') appendFlickPi(cid);
  } else if (bt === 'btn2') {
    if (dir === 'left') appendFlickChar('P', cid);
    else if (dir === 'right') appendFlickChar('C', cid);
  }
  flickState.direction = null;
  flickState.calcId = null;
  flickState.btnType = null;
});
document.addEventListener('mousemove', (e) => {
  if (!flickState.active || flickState.calcId === null) return;
  const dx = e.clientX - flickState.startX;
  const dy = e.clientY - flickState.startY;
  if (Math.abs(dx) > 15 || Math.abs(dy) > 15) {
    clearTimeout(flickState.longPressTimer);
  }
  let dir = null;
  if (Math.abs(dx) > Math.abs(dy) + 10) {
    dir = dx < 0 ? 'left' : 'right';
  }
  flickState.direction = dir;
  if (flickState.popupVisible) updateFlickPopup(dir, flickState.btnType);
});
// 初期化時にフリックを設定
(function() {
  setupDotButtonFlick(1);
  setupDotButtonFlick(2);
  setupCaretButtonFlick(1);
  setupCaretButtonFlick(2);
  setup3ButtonFlick(1);
  setup3ButtonFlick(2);
  setup2ButtonFlick(1);
  setup2ButtonFlick(2);
  // i18n 初期化
  buildLangButtons();
  applyLang(currentLang);
})();
/* ===== Ripple Animation Script ===== */
document.addEventListener('touchstart', (e) => {
  const btn = e.target.closest('.calc-btn');
  if (!btn) return;
  const ripple = document.createElement('span');
  ripple.classList.add('ripple');
  
  const rect = btn.getBoundingClientRect();
  const size = Math.max(rect.width, rect.height);
  const x = e.touches[0].clientX - rect.left - size / 2;
  const y = e.touches[0].clientY - rect.top - size / 2;
  
  ripple.style.width = ripple.style.height = `${size}px`;
  ripple.style.left = `${x}px`;
  ripple.style.top = `${y}px`;
  
  btn.appendChild(ripple);
  
  ripple.addEventListener('animationend', () => {
    ripple.remove();
  });
}, { passive: true });
document.addEventListener('mousedown', (e) => {
  if ('ontouchstart' in window) return;
  const btn = e.target.closest('.calc-btn');
  if (!btn) return;
  const ripple = document.createElement('span');
  ripple.classList.add('ripple');
  
  const rect = btn.getBoundingClientRect();
  const size = Math.max(rect.width, rect.height);
  const x = e.clientX - rect.left - size / 2;
  const y = e.clientY - rect.top - size / 2;
  
  ripple.style.width = ripple.style.height = `${size}px`;
  ripple.style.left = `${x}px`;
  ripple.style.top = `${y}px`;
  
  btn.appendChild(ripple);
  
  ripple.addEventListener('animationend', () => {
    ripple.remove();
  });
});
</script>
</body>
</html>
