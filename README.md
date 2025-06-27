import React, { useState, useEffect } from 'react';
import { Calculator, CreditCard, Coins } from 'lucide-react';

export default function CreditCardRewardCalculator() {
  const [transactionValue, setTransactionValue] = useState('1000');
  const [pointValue, setPointValue] = useState('1');
  const [rewardPoints, setRewardPoints] = useState('10');
  const [spendingThreshold, setSpendingThreshold] = useState('200');
  
  const [results, setResults] = useState({
    totalPoints: 0,
    totalRewardValue: 0,
    effectiveRewardRate: 0
  });

  useEffect(() => {
    calculateRewards();
  }, [transactionValue, pointValue, rewardPoints, spendingThreshold]);

  const calculateRewards = () => {
    const transaction = parseFloat(transactionValue) || 0;
    const perPointValue = parseFloat(pointValue) || 0;
    const rewardPointsNum = parseFloat(rewardPoints) || 0;
    const spendThreshold = parseFloat(spendingThreshold) || 1;

    // Calculate total points earned
    const totalPoints = Math.floor((transaction / spendThreshold) * rewardPointsNum);
    
    // Calculate total reward value in whole rupees only
    const totalRewardValue = Math.floor(totalPoints * perPointValue);
    
    // Calculate effective reward rate as percentage
    const effectiveRewardRate = transaction > 0 ? (totalRewardValue / transaction) * 100 : 0;

    setResults({
      totalPoints,
      totalRewardValue,
      effectiveRewardRate: Math.round(effectiveRewardRate * 100) / 100
    });
  };

  const presetCards = [
    { name: 'HDFC Millennia', points: '10', threshold: '200', value: '0.25' },
    { name: 'SBI SimplyCLICK', points: '10', threshold: '100', value: '0.25' },
    { name: 'ICICI Amazon Pay', points: '1', threshold: '100', value: '100' },
    { name: 'Axis Flipkart', points: '4', threshold: '200', value: '0.25' },
    { name: 'Custom', points: rewardPoints, threshold: spendingThreshold, value: pointValue }
  ];

  const loadPreset = (preset) => {
    if (preset.name !== 'Custom') {
      setRewardPoints(preset.points);
      setSpendingThreshold(preset.threshold);
      setPointValue(preset.value);
    }
  };

  const transactionPresets = ['500', '1000', '2000', '5000', '10000'];

  return (
    <div className="min-h-screen bg-gradient-to-br from-blue-50 to-indigo-100 p-4">
      <div className="max-w-4xl mx-auto">
        <div className="bg-white rounded-3xl shadow-xl p-8">
          <div className="text-center mb-8">
            <div className="flex justify-center items-center gap-3 mb-4">
              <CreditCard className="h-8 w-8 text-indigo-600" />
              <Calculator className="h-8 w-8 text-indigo-600" />
            </div>
            <h1 className="text-3xl font-bold text-gray-800 mb-2">
              Credit Card Reward Calculator
            </h1>
            <p className="text-gray-600">
              Calculate your rewards in whole rupees - Simple & Fast
            </p>
          </div>

          <div className="grid lg:grid-cols-2 gap-8">
            {/* Input Section */}
            <div className="space-y-6">              
              {/* Transaction Amount */}
              <div>
                <label className="block text-lg font-semibold text-gray-700 mb-3">
                  Transaction Amount (₹)
                </label>
                <div className="space-y-3">
                  <input
                    type="number"
                    value={transactionValue}
                    onChange={(e) => setTransactionValue(e.target.value)}
                    className="w-full px-4 py-3 text-lg border-2 border-gray-300 rounded-xl focus:ring-2 focus:ring-indigo-500 focus:border-indigo-500 transition-all"
                    placeholder="Enter amount"
                  />
                  <div className="flex flex-wrap gap-2">
                    {transactionPresets.map(amount => (
                      <button
                        key={amount}
                        onClick={() => setTransactionValue(amount)}
                        className={`px-4 py-2 rounded-lg border transition-all ${
                          transactionValue === amount 
                            ? 'bg-indigo-600 text-white border-indigo-600' 
                            : 'bg-gray-50 text-gray-700 border-gray-300 hover:bg-indigo-50'
                        }`}
                      >
                        ₹{amount}
                      </button>
                    ))}
                  </div>
                </div>
              </div>

              {/* Card Dropdown */}
              <div>
                <label className="block text-lg font-semibold text-gray-700 mb-3">
                  Select Credit Card
                </label>
                <select
                  onChange={(e) => {
                    const selectedCard = presetCards.find(card => card.name === e.target.value);
                    if (selectedCard) loadPreset(selectedCard);
                  }}
                  className="w-full px-4 py-3 text-lg border-2 border-gray-300 rounded-xl focus:ring-2 focus:ring-indigo-500 focus:border-indigo-500 transition-all bg-white"
                  defaultValue="Custom"
                >
                  {presetCards.map(card => (
                    <option key={card.name} value={card.name}>
                      {card.name === 'Custom' ? 
                        'Custom Card Settings' : 
                        `${card.name} - ${card.points} pts per ₹${card.threshold}`
                      }
                    </option>
                  ))}
                </select>
              </div>

              {/* Dynamic Reward Structure */}
              <div className="bg-indigo-50 rounded-2xl p-6 border-2 border-indigo-200">
                <label className="block text-lg font-semibold text-gray-700 mb-4">
                  <Coins className="inline h-5 w-5 mr-2" />
                  Reward Structure
                </label>
                <div className="bg-white rounded-xl p-4 border border-indigo-200">
                  <div className="text-center text-lg text-gray-700 mb-4">
                    <span className="text-gray-600">Earn </span>
                    <input
                      type="number"
                      value={rewardPoints}
                      onChange={(e) => setRewardPoints(e.target.value)}
                      className="w-16 px-2 py-1 mx-1 text-center text-xl font-bold text-indigo-600 border-2 border-indigo-300 rounded-lg focus:ring-2 focus:ring-indigo-500 focus:border-indigo-500"
                    />
                    <span className="text-gray-600"> points on every ₹</span>
                    <input
                      type="number"
                      value={spendingThreshold}
                      onChange={(e) => setSpendingThreshold(e.target.value)}
                      className="w-20 px-2 py-1 mx-1 text-center text-xl font-bold text-indigo-600 border-2 border-indigo-300 rounded-lg focus:ring-2 focus:ring-indigo-500 focus:border-indigo-500"
                    />
                    <span className="text-gray-600"> spent</span>
                  </div>
                  
                  <div className="text-center">
                    <span className="text-gray-600">Each point = ₹</span>
                    <input
                      type="number"
                      step="0.01"
                      value={pointValue}
                      onChange={(e) => setPointValue(e.target.value)}
                      className="w-24 px-2 py-1 mx-1 text-center text-xl font-bold text-green-600 border-2 border-green-300 rounded-lg focus:ring-2 focus:ring-green-500 focus:border-green-500"
                    />
                  </div>
                </div>
              </div>
            </div>

            {/* Results Section */}
            <div className="space-y-6">
              {/* Main Results */}
              <div className="bg-gradient-to-br from-green-50 to-emerald-50 rounded-2xl p-6 border-2 border-green-200">
                <h2 className="text-xl font-semibold text-gray-800 mb-6 text-center">
                  💰 Your Rewards
                </h2>
                
                <div className="space-y-6">
                  <div className="text-center bg-white rounded-xl p-4 border border-green-200">
                    <div className="text-sm text-gray-600 mb-1">Points Earned</div>
                    <div className="text-3xl font-bold text-blue-600">
                      {results.totalPoints.toLocaleString()}
                    </div>
                    <div className="text-xs text-gray-500 mt-1">points</div>
                  </div>
                  
                  <div className="text-center bg-white rounded-xl p-4 border border-green-200">
                    <div className="text-sm text-gray-600 mb-1">Reward Value</div>
                    <div className="text-4xl font-bold text-green-600">
                      ₹{results.totalRewardValue.toLocaleString()}
                    </div>
                    <div className="text-xs text-gray-500 mt-1">in whole rupees</div>
                  </div>
                  
                  <div className="text-center bg-white rounded-xl p-4 border border-green-200">
                    <div className="text-sm text-gray-600 mb-1">Reward Rate</div>
                    <div className="text-3xl font-bold text-purple-600">
                      {results.effectiveRewardRate}%
                    </div>
                    <div className="text-xs text-gray-500 mt-1">return on spending</div>
                  </div>
                </div>
              </div>

              {/* Quick Calculation */}
              <div className="bg-gray-50 rounded-2xl p-6 border border-gray-200">
                <h3 className="font-semibold text-gray-700 mb-4 text-center">
                  📊 How it's calculated
                </h3>
                <div className="space-y-3 text-sm">
                  <div className="bg-white p-3 rounded-lg border">
                    <div className="font-medium text-gray-700">Step 1: Calculate Points</div>
                    <div className="text-gray-600">
                      ₹{transactionValue || 0} ÷ ₹{spendingThreshold || 1} × {rewardPoints || 0} = <span className="font-bold text-blue-600">{results.totalPoints} points</span>
                    </div>
                  </div>
                  
                  <div className="bg-white p-3 rounded-lg border">
                    <div className="font-medium text-gray-700">Step 2: Convert to Rupees</div>
                    <div className="text-gray-600">
                      {results.totalPoints} points × ₹{pointValue || 0} = <span className="font-bold text-green-600">₹{results.totalRewardValue}</span>
                    </div>
                  </div>
                  
                  <div className="bg-white p-3 rounded-lg border">
                    <div className="font-medium text-gray-700">Step 3: Calculate Rate</div>
                    <div className="text-gray-600">
                      ₹{results.totalRewardValue} ÷ ₹{transactionValue || 1} × 100 = <span className="font-bold text-purple-600">{results.effectiveRewardRate}%</span>
                    </div>
                  </div>
                </div>
              </div>

              {/* Quick Tips */}
              <div className="bg-blue-50 rounded-2xl p-6 border border-blue-200">
                <h3 className="font-semibold text-gray-700 mb-3 text-center">
                  💡 Quick Guide
                </h3>
                <div className="text-sm text-gray-700 space-y-2">
                  <div className="flex items-center gap-2">
                    <div className="w-2 h-2 bg-green-500 rounded-full"></div>
                    <span><strong>Excellent:</strong> 2%+ reward rate</span>
                  </div>
                  <div className="flex items-center gap-2">
                    <div className="w-2 h-2 bg-yellow-500 rounded-full"></div>
                    <span><strong>Good:</strong> 1-2% reward rate</span>
                  </div>
                  <div className="flex items-center gap-2">
                    <div className="w-2 h-2 bg-red-500 rounded-full"></div>
                    <span><strong>Average:</strong> Below 1% reward rate</span>
                  </div>
                  <div className="text-xs text-gray-500 mt-2 p-2 bg-white rounded">
                    <strong>Note:</strong> Only whole rupees are calculated and displayed. Fractional amounts are rounded down.
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  );
}
