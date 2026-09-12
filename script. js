const readout = document.getElementById('readout');
const trail = document.getElementById('trail');

let current = '0';
let stored = null;
let pendingOp = null;
let overwrite = true;

const symbols = { add: '+', subtract: '−', multiply: '×', divide: '÷' };

function render() {
  readout.textContent = current;
  trail.textContent = stored !== null && pendingOp
    ? `${formatDisplay(stored)} ${symbols[pendingOp]}`
    : '';
}

function formatDisplay(n) {
  const str = String(n);
  return str.length > 12 ? Number(n).toExponential(4) : str;
}

function inputDigit(d) {
  if (overwrite) {
    current = d === '.' ? '0.' : d;
    overwrite = false;
  } else {
    if (d === '.' && current.includes('.')) return;
    if (current.replace('-', '').replace('.', '').length >= 12) return;
    current += d;
  }
}

function compute(a, b, op) {
  switch (op) {
    case 'add': return a + b;
    case 'subtract': return a - b;
    case 'multiply': return a * b;
    case 'divide': return b === 0 ? NaN : a / b;
  }
}

function setOperator(op) {
  const value = parseFloat(current);
  if (pendingOp && !overwrite) {
    stored = compute(stored, value, pendingOp);
    current = formatDisplay(round(stored));
  } else {
    stored = value;
  }
  pendingOp = op;
  overwrite = true;
}

function round(n) {
  return Math.round((n + Number.EPSILON) * 1e10) / 1e10;
}

function equals() {
  if (pendingOp === null) return;
  const value = parseFloat(current);
  const result = round(compute(stored, value, pendingOp));
  current = Number.isNaN(result) ? 'Error' : formatDisplay(result);
  stored = null;
  pendingOp = null;
  overwrite = true;
}

function clearAll() {
  current = '0';
  stored = null;
  pendingOp = null;
  overwrite = true;
}

function toggleSign() {
  if (current === '0') return;
  current = current.startsWith('-') ? current.slice(1) : '-' + current;
}

function percent() {
  current = formatDisplay(round(parseFloat(current) / 100));
  overwrite = false;
}

document.querySelectorAll('button').forEach(btn => {
  btn.addEventListener('click', () => {
    const { digit, action } = btn.dataset;
    if (digit !== undefined) {
      inputDigit(digit);
    } else if (action === 'decimal') {
      inputDigit('.');
    } else if (['add', 'subtract', 'multiply', 'divide'].includes(action)) {
      setOperator(action);
    } else if (action === 'equals') {
      equals();
    } else if (action === 'clear') {
      clearAll();
    } else if (action === 'sign') {
      toggleSign();
    } else if (action === 'percent') {
      percent();
    }
    render();
  });
});

window.addEventListener('keydown', (e) => {
  if (/[0-9]/.test(e.key)) inputDigit(e.key);
  else if (e.key === '.') inputDigit('.');
  else if (e.key === '+') setOperator('add');
  else if (e.key === '-') setOperator('subtract');
  else if (e.key === '*') setOperator('multiply');
  else if (e.key === '/') { e.preventDefault(); setOperator('divide'); }
  else if (e.key === 'Enter' || e.key === '=') equals();
  else if (e.key === 'Escape') clearAll();
  else if (e.key === 'Backspace') {
    current = current.length > 1 ? current.slice(0, -1) : '0';
    overwrite = current === '0';
  } else return;
  render();
});

render();
