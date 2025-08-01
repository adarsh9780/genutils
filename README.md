```javascript
import React from 'react';
import { FaRegCopy } from 'react-icons/fa';

export const PromptLibraryModal = ({ isOpen, prompts, onClose }) => {
  const copyToClipboard = async (text) => {
    try {
      await navigator.clipboard.writeText(text);
      alert('Copied to clipboard!');
    } catch (err) {
      console.error('Failed to copy:', err);
    }
  };

  if (!isOpen) return null;

  return (
    <div className="fixed inset-0 flex items-center justify-center bg-black bg-opacity-40 z-50">
      <div className="bg-white w-full max-w-lg rounded-2xl shadow-lg p-6">
        <div className="flex justify-between items-center mb-4">
          <h2 className="text-xl font-semibold">Prompt Library</h2>
          <button onClick={onClose} className="text-gray-500 hover:text-gray-700">✕</button>
        </div>

        <div className="space-y-4 max-h-[60vh] overflow-y-auto">
          {prompts.map((prompt, index) => (
            <div key={index} className="flex justify-between items-start gap-2 border p-3 rounded-lg bg-gray-50">
              <p className="text-sm text-gray-800 flex-1">{prompt}</p>
              <button
                onClick={() => copyToClipboard(prompt)}
                className="text-blue-600 hover:text-blue-800"
                title="Copy prompt"
              >
                <FaRegCopy />
              </button>
            </div>
          ))}
        </div>
      </div>
    </div>
  );
};
```
