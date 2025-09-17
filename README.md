# symmetrical-rotary-phone
Budget calculator 3
{
  "name": "budget-calculator",
  "version": "1.0.0",
  "private": true,
  "dependencies": {
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
    "react-scripts": "5.0.1",
    "tailwindcss": "^3.3.2"
  },
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject"
  }
}/** @type {import('tailwindcss').Config} */
module.exports = {
  content: [
    "./src/**/*.{js,jsx,ts,tsx}"
  ],
  theme: {
    extend: {},
  },
  plugins: [],
}@tailwind base;
@tailwind components;
@tailwind utilities;

body {
  @apply bg-gray-100 text-gray-800;
}import React from 'react';
import ReactDOM from 'react-dom/client';
import './index.css';
import App from './App';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);import React, { useState } from 'react';
import TransactionForm from './components/TransactionForm';
import TransactionList from './components/TransactionList';
import Summary from './components/Summary';

function App() {
  const [transactions, setTransactions] = useState([]);

  const addTransaction = (transaction) => {
    setTransactions([...transactions, transaction]);
  };

  return (
    <div className="max-w-3xl mx-auto p-6">
      <h1 className="text-3xl font-bold mb-6 text-center">Budget Calculator</h1>
      <TransactionForm addTransaction={addTransaction} />
      <Summary transactions={transactions} />
      <TransactionList transactions={transactions} />
    </div>
  );
}

export default App;import React, { useState } from 'react';

function TransactionForm({ addTransaction }) {
  const [title, setTitle] = useState('');
  const [amount, setAmount] = useState('');
  const [type, setType] = useState('income');

  const handleSubmit = (e) => {
    e.preventDefault();
    if (!title || !amount) return;

    addTransaction({
      id: Date.now(),
      title,
      amount: parseFloat(amount),
      type
    });

    setTitle('');
    setAmount('');
  };

  return (
    <form className="mb-6 p-4 bg-white rounded shadow" onSubmit={handleSubmit}>
      <div className="mb-4">
        <label className="block mb-1 font-medium">Title</label>
        <input
          type="text"
          className="w-full border rounded p-2"
          value={title}
          onChange={(e) => setTitle(e.target.value)}
        />
      </div>

      <div className="mb-4">
        <label className="block mb-1 font-medium">Amount</label>
        <input
          type="number"
          className="w-full border rounded p-2"
          value={amount}
          onChange={(e) => setAmount(e.target.value)}
        />
      </div>

      <div className="mb-4">
        <label className="block mb-1 font-medium">Type</label>
        <select
          className="w-full border rounded p-2"
          value={type}
          onChange={(e) => setType(e.target.value)}
        >
          <option value="income">Income</option>
          <option value="expense">Expense</option>
        </select>
      </div>

      <button
        type="submit"
        className="w-full bg-blue-500 text-white p-2 rounded hover:bg-blue-600"
      >
        Add Transaction
      </button>
    </form>
  );
}

export default TransactionForm;import React from 'react';

function TransactionList({ transactions }) {
  if (transactions.length === 0) {
    return <p className="text-center text-gray-500">No transactions yet.</p>;
  }

  return (
    <div className="mb-6 p-4 bg-white rounded shadow">
      <h2 className="text-xl font-bold mb-4">Transactions</h2>
      <ul>
        {transactions.map((t) => (
          <li
            key={t.id}
            className={`flex justify-between p-2 border-b ${t.type === 'income' ? 'text-green-600' : 'text-red-600'}`}
          >
            <span>{t.title}</span>
            <span>{t.type === 'income' ? '+' : '-'}${t.amount.toFixed(2)}</span>
          </li>
        ))}
      </ul>
    </div>
  );
}

export default TransactionList;import React from 'react';

function Summary({ transactions }) {
  const income = transactions
    .filter(t => t.type === 'income')
    .reduce((sum, t) => sum + t.amount, 0);

  const expense = transactions
    .filter(t => t.type === 'expense')
    .reduce((sum, t) => sum + t.amount, 0);

  const balance = income - expense;

  return (
    <div className="mb-6 p-4 bg-white rounded shadow flex justify-around text-center">
      <div>
        <h3 className="font-bold">Income</h3>
        <p className="text-green-600">${income.toFixed(2)}</p>
      </div>
      <div>
        <h3 className="font-bold">Expense</h3>
        <p className="text-red-600">${expense.toFixed(2)}</p>
      </div>
      <div>
        <h3 className="font-bold">Balance</h3>
        <p className={`${balance >= 0 ? 'text-green-600' : 'text-red-600'}`}>
          ${balance.toFixed(2)}
        </p>
      </div>
    </div>
  );
}

export default Summary;
