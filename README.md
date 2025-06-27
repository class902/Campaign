import React, { useState, useEffect } from 'react';
import { Calculator, CreditCard, Save, Copy, RotateCcw, Plus, Trash2, Info } from 'lucide-react';

const CreditCardCalculator = () => {
  const [rewardPoints, setRewardPoints] = useState(10);
  const [spendingThreshold, setSpendingThreshold] = useState(200);
  const [annualFee, setAnnualFee] = useState(0);
  const [monthlySpending, setMonthlySpending] = useState(15000);
  const [currentTransaction, setCurrentTransaction] = useState(1000);
  const [cardName, setCardName] = useState('');
  const [savedCards, setSavedCards] = useState([]);
  const [showComparison, setShowComparison] = useState(false);
  const [copySuccess, setCopySuccess] = useState(false);

  // Load saved cards from localStorage on component mount
  useEffect(() => {
    const saved = localStorage.getItem('savedCreditCards');
    if (saved) {
      setSavedCards(JSON.parse(saved));
    }
  }, []);

  // Save cards to localStorage whenever savedCards changes
  useEffect(() => {
    localStorage.setItem('savedCreditCards', JSON.stringify(savedCards));
  }, [savedCards]);

  // Calculations
  const rewardRate = rewardPoints / spendingThreshold;
  const currentReward = Math.floor(currentTransaction / spendingThreshold) * rewardPoints;
  const annualSpending = monthlySpending * 12;
  const annualRewardPoints = Math.floor(annualSpending / spendingThreshold) * rewardPoints;
  const rewardValue = annualRewardPoints * 0.25; // Assuming 1 point = ₹0.25
  const netAnnualBenefit = rewardValue - annualFee;
  const breakEvenSpending = annualFee > 0 ? (annualFee / (rewardPoints * 0.25)) * spendingThreshold : 0;

  const saveCard = () => {
    if (!cardName.trim()) {
      alert('Please enter a card name');
      return;
    }
    
    const newCard = {
      id: Date.now(),
      name: cardName,
      rewardPoints,
      spendingThreshold,
      annualFee,
      rewardRate,
      netBenefit: netAnnualBenefit
    };
    
    setSavedCards([...savedCards, newCard]);
    setCardName('');
    alert('Card saved successfully!');
  };

  const loadCard = (card) => {
    setRewardPoints(card.rewardPoints);
    setSpendingThreshold(card.spendingThreshold);
    setAnnualFee(card.annualFee);
    setCardName(card.name);
  };

  const deleteCard = (cardId) => {
    setSavedCards(savedCards.filter(card => card.id !== cardId));
  };

  const copyResults = () => {
    const results = `
Credit Card Analysis: ${cardName || 'Current Card'}
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📊 REWARD STRUCTURE
• Earn ${rewardPoints} points per ₹${spendingThreshold} spent
• Reward Rate: ${(rewardRate * 100).toFixed(2)}%
• Annual Fee: ₹${annualFee.toLocaleString()}

💰 CURRENT TRANSACTION
• Transaction Amount: ₹${currentTransaction.toLocaleString()}
• Reward Points: ${currentReward}
• Reward Value: ₹${(currentReward * 0.25).toFixed(2)}

📈 ANNUAL PROJECTION
• Monthly Spending: ₹${monthlySpending.toLocaleString()}
• Annual Spending: ₹${annualSpending.toLocaleString()}
• Annual Reward Points: ${annualRewardPoints.toLocaleString()}
• Reward Value: ₹${rewardValue.toFixed(2)}
• Net Annual Benefit: ₹${netAnnualBenefit.toFixed(2)}

${breakEvenSpending > 0 ? `⚖️ BREAK-EVEN ANALYSIS
• Break-even Spending: ₹${breakEvenSpending.toLocaleString()} annually` : ''}
    `;
    
    navigator.clipboard.writeText(results.trim());
    setCopySuccess(true);
    setTimeout(() => setCopySuccess(false), 2000);
  };

  const resetForm = () => {
    setRewardPoints(10);
    setSpendingThreshold(200);
    setAnnualFee(0);
    setMonthlySpending(15000);
    setCurrentTransaction(1000);
    setCardName('');
  };

  const Tooltip = ({ children, text }) => (
    <div className="relative group">
      {children}
      <div className="absolute bottom-full left-1/2 transform -translate-x-1/2 mb-2 px-3 py-1 bg-gray-800 text-white text-xs rounded-lg opacity-0 group-hover:opacity-100 transition-opacity duration-200 whitespace-nowrap z-10">
        {text}
      </div>
    </div>
  );

  return (
    <div className="max-w-6xl mx-auto p-6 bg-gradient-to-br from-blue-50 to-indigo-100 min-h-screen">
      <div className="text-center mb-8">
        <h1 className="text-4xl font-bold text-gray-800 mb-2 flex items-center justify-center gap-3">
          <CreditCard className="text-blue-600" />
          Credit Card Rewards Calculator
        </h1>
        <p className="text-gray-600">Make informed decisions with comprehensive credit card analysis</p>
      </div>

      <div className="grid grid-cols-1 lg:grid-cols-2 gap-8">
        {/* Input Section */}
        <div className="bg-white rounded-xl shadow-lg p-6">
          <h2 className="text-2xl font-semibold text-gray-800 mb-6 flex items-center gap-2">
            <Calculator className="text-blue-600" />
            Card Configuration
          </h2>

          <div className="space-y-6">
            {/* Card Name */}
            <div>
              <label className="block text-sm font-medium text-gray-700 mb-2">
                Card Name (Optional)
              </label>
              <input
                type="text"
                value={cardName}
                onChange={(e) => setCardName(e.target.value)}
                placeholder="e.g., HDFC Regalia Gold"
                className="w-full px-4 py-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
              />
            </div>

            {/* Reward Structure */}
            <div className="bg-blue-50 p-4 rounded-lg">
              <label className="block text-sm font-medium text-gray-700 mb-3">
                Reward Structure
              </label>
              <div className="flex items-center gap-2 text-lg">
                <span>Earn</span>
                <input
                  type="number"
                  value={rewardPoints}
                  onChange={(e) => setRewardPoints(Number(e.target.value))}
                  className="w-16 px-2 py-1 border border-gray-300 rounded text-center font-semibold"
                  min="1"
                />
                <span>reward points on every ₹</span>
                <input
                  type="number"
                  value={spendingThreshold}
                  onChange={(e) => setSpendingThreshold(Number(e.target.value))}
                  className="w-20 px-2 py-1 border border-gray-300 rounded text-center font-semibold"
                  min="1"
                />
                <span>spending</span>
              </div>
              <div className="text-sm text-gray-600 mt-2">
                Reward Rate: {(rewardRate * 100).toFixed(2)}% value back
              </div>
            </div>

            {/* Annual Fee */}
            <div>
              <label className="block text-sm font-medium text-gray-700 mb-2 flex items-center gap-1">
                Annual Fee (₹)
                <Tooltip text="Annual fee charged by the bank">
                  <Info className="w-4 h-4 text-gray-400" />
                </Tooltip>
              </label>
              <input
                type="number"
                value={annualFee}
                onChange={(e) => setAnnualFee(Number(e.target.value))}
                className="w-full px-4 py-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
                min="0"
              />
            </div>

            {/* Monthly Spending */}
            <div>
              <label className="block text-sm font-medium text-gray-700 mb-2 flex items-center gap-1">
                Monthly Spending (₹)
                <Tooltip text="Your average monthly spending on this card">
                  <Info className="w-4 h-4 text-gray-400" />
                </Tooltip>
              </label>
              <input
                type="number"
                value={monthlySpending}
                onChange={(e) => setMonthlySpending(Number(e.target.value))}
                className="w-full px-4 py-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
                min="0"
              />
            </div>

            {/* Current Transaction */}
            <div>
              <label className="block text-sm font-medium text-gray-700 mb-2">
                Current Transaction (₹)
              </label>
              <input
                type="number"
                value={currentTransaction}
                onChange={(e) => setCurrentTransaction(Number(e.target.value))}
                className="w-full px-4 py-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
                min="0"
              />
            </div>

            {/* Action Buttons */}
            <div className="flex gap-3 pt-4">
              <button
                onClick={saveCard}
                className="flex items-center gap-2 px-4 py-2 bg-green-600 text-white rounded-lg hover:bg-green-700 transition-colors"
              >
                <Save className="w-4 h-4" />
                Save Card
              </button>
              <button
                onClick={resetForm}
                className="flex items-center gap-2 px-4 py-2 bg-gray-600 text-white rounded-lg hover:bg-gray-700 transition-colors"
              >
                <RotateCcw className="w-4 h-4" />
                Reset
              </button>
            </div>
          </div>
        </div>

        {/* Results Section */}
        <div className="bg-white rounded-xl shadow-lg p-6">
          <div className="flex items-center justify-between mb-6">
            <h2 className="text-2xl font-semibold text-gray-800">Analysis Results</h2>
            <button
              onClick={copyResults}
              className={`flex items-center gap-2 px-4 py-2 rounded-lg transition-colors ${
                copySuccess 
                  ? 'bg-green-600 text-white' 
                  : 'bg-blue-600 text-white hover:bg-blue-700'
              }`}
            >
              <Copy className="w-4 h-4" />
              {copySuccess ? 'Copied!' : 'Copy Results'}
            </button>
          </div>

          <div className="space-y-6">
            {/* Current Transaction */}
            <div className="bg-gray-50 p-4 rounded-lg">
              <h3 className="font-semibold text-gray-800 mb-3">Current Transaction</h3>
              <div className="grid grid-cols-2 gap-4 text-sm">
                <div>
                  <span className="text-gray-600">Amount:</span>
                  <div className="font-semibold text-lg">₹{currentTransaction.toLocaleString()}</div>
                </div>
                <div>
                  <span className="text-gray-600">Reward Points:</span>
                  <div className="font-semibold text-lg text-green-600">{currentReward}</div>
                </div>
              </div>
              <div className="mt-2">
                <span className="text-gray-600">Reward Value:</span>
                <div className="font-semibold text-lg text-green-600">₹{(currentReward * 0.25).toFixed(2)}</div>
              </div>
            </div>

            {/* Annual Projection */}
            <div className="bg-blue-50 p-4 rounded-lg">
              <h3 className="font-semibold text-gray-800 mb-3">Annual Projection</h3>
              <div className="space-y-2 text-sm">
                <div className="flex justify-between">
                  <span className="text-gray-600">Annual Spending:</span>
                  <span className="font-semibold">₹{annualSpending.toLocaleString()}</span>
                </div>
                <div className="flex justify-between">
                  <span className="text-gray-600">Annual Reward Points:</span>
                  <span className="font-semibold text-green-600">{annualRewardPoints.toLocaleString()}</span>
                </div>
                <div className="flex justify-between">
                  <span className="text-gray-600">Reward Value:</span>
                  <span className="font-semibold text-green-600">₹{rewardValue.toFixed(2)}</span>
                </div>
                <div className="flex justify-between">
                  <span className="text-gray-600">Annual Fee:</span>
                  <span className="font-semibold text-red-600">-₹{annualFee.toLocaleString()}</span>
                </div>
                <hr className="my-2" />
                <div className="flex justify-between text-lg">
                  <span className="font-semibold">Net Annual Benefit:</span>
                  <span className={`font-bold ${netAnnualBenefit >= 0 ? 'text-green-600' : 'text-red-600'}`}>
                    ₹{netAnnualBenefit.toFixed(2)}
                  </span>
                </div>
              </div>
            </div>

            {/* Break-even Analysis */}
            {annualFee > 0 && (
              <div className="bg-yellow-50 p-4 rounded-lg">
                <h3 className="font-semibold text-gray-800 mb-3">Break-even Analysis</h3>
                <div className="text-sm">
                  <span className="text-gray-600">Minimum annual spending to justify fee:</span>
                  <div className="font-semibold text-lg text-orange-600">₹{breakEvenSpending.toLocaleString()}</div>
                  <div className="text-xs text-gray-500 mt-1">
                    Monthly: ₹{(breakEvenSpending / 12).toLocaleString()}
                  </div>
                </div>
              </div>
            )}

            {/* Quick Tips */}
            <div className="bg-indigo-50 p-4 rounded-lg">
              <h3 className="font-semibold text-gray-800 mb-2">💡 Quick Tips</h3>
              <ul className="text-sm text-gray-600 space-y-1">
                <li>• Cards with annual fees need higher spending to be worthwhile</li>
                <li>• Compare net annual benefit across different cards</li>
                <li>• Consider category-specific bonus rates for better rewards</li>
                <li>• Factor in redemption options and point values</li>
              </ul>
            </div>
          </div>
        </div>
      </div>

      {/* Saved Cards Section */}
      {savedCards.length > 0 && (
        <div className="mt-8 bg-white rounded-xl shadow-lg p-6">
          <div className="flex items-center justify-between mb-6">
            <h2 className="text-2xl font-semibold text-gray-800">Saved Cards</h2>
            <button
              onClick={() => setShowComparison(!showComparison)}
              className="px-4 py-2 bg-indigo-600 text-white rounded-lg hover:bg-indigo-700 transition-colors"
            >
              {showComparison ? 'Hide' : 'Show'} Comparison
            </button>
          </div>

          {showComparison && (
            <div className="overflow-x-auto mb-6">
              <table className="w-full border-collapse border border-gray-300">
                <thead>
                  <tr className="bg-gray-50">
                    <th className="border border-gray-300 px-4 py-2 text-left">Card Name</th>
                    <th className="border border-gray-300 px-4 py-2 text-left">Reward Rate</th>
                    <th className="border border-gray-300 px-4 py-2 text-left">Annual Fee</th>
                    <th className="border border-gray-300 px-4 py-2 text-left">Net Benefit</th>
                  </tr>
                </thead>
                <tbody>
                  {savedCards.map((card) => (
                    <tr key={card.id} className="hover:bg-gray-50">
                      <td className="border border-gray-300 px-4 py-2 font-medium">{card.name}</td>
                      <td className="border border-gray-300 px-4 py-2">{(card.rewardRate * 100).toFixed(2)}%</td>
                      <td className="border border-gray-300 px-4 py-2">₹{card.annualFee.toLocaleString()}</td>
                      <td className={`border border-gray-300 px-4 py-2 font-semibold ${
                        card.netBenefit >= 0 ? 'text-green-600' : 'text-red-600'
                      }`}>
                        ₹{card.netBenefit.toFixed(2)}
                      </td>
                    </tr>
                  ))}
                </tbody>
              </table>
            </div>
          )}

          <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
            {savedCards.map((card) => (
              <div key={card.id} className="border border-gray-200 rounded-lg p-4 hover:border-blue-300 transition-colors">
                <div className="flex items-center justify-between mb-2">
                  <h3 className="font-semibold text-gray-800 truncate">{card.name}</h3>
                  <button
                    onClick={() => deleteCard(card.id)}
                    className="text-red-500 hover:text-red-700 transition-colors"
                  >
                    <Trash2 className="w-4 h-4" />
                  </button>
                </div>
                <div className="text-sm text-gray-600 space-y-1">
                  <div>Rate: {(card.rewardRate * 100).toFixed(2)}%</div>
                  <div>Fee: ₹{card.annualFee.toLocaleString()}</div>
                  <div className={`font-semibold ${card.netBenefit >= 0 ? 'text-green-600' : 'text-red-600'}`}>
                    Net: ₹{card.netBenefit.toFixed(2)}
                  </div>
                </div>
                <button
                  onClick={() => loadCard(card)}
                  className="mt-3 w-full px-3 py-1 bg-blue-600 text-white text-sm rounded hover:bg-blue-700 transition-colors"
                >
                  Load Card
                </button>
              </div>
            ))}
          </div>
        </div>
      )}
    </div>
  );
};

export default CreditCardCalculator;
