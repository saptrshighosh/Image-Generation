import React, { useState, useEffect } from 'react';

// Main App Component
export default function App() {
  const [prompt, setPrompt] = useState('');
  const [imageUrl, setImageUrl] = useState('');
  const [isLoading, setIsLoading] = useState(false);
  const [error, setError] = useState('');
  const [showModal, setShowModal] = useState(false);
  const [modalMessage, setModalMessage] = useState('');

  // Function to display modal messages
  const displayModal = (message) => {
    setModalMessage(message);
    setShowModal(true);
  };

  // Function to handle image generation
  const generateImage = async () => {
    if (!prompt.trim()) {
      displayModal('Please enter a prompt for the image.');
      return;
    }

    setIsLoading(true);
    setImageUrl('');
    setError('');

    const payload = {
      instances: [{ prompt: prompt }],
      parameters: { sampleCount: 1 },
    };
    const apiKey = ""; // API key will be injected by the environment
    const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/imagen-3.0-generate-002:predict?key=${apiKey}`;

    try {
      const response = await fetch(apiUrl, {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(payload),
      });

      if (!response.ok) {
        const errorData = await response.json();
        console.error('Error response from API:', errorData);
        throw new Error(`API request failed with status ${response.status}: ${errorData.error?.message || 'Unknown error'}`);
      }

      const result = await response.json();

      if (result.predictions && result.predictions.length > 0 && result.predictions[0].bytesBase64Encoded) {
        setImageUrl(`data:image/png;base64,${result.predictions[0].bytesBase64Encoded}`);
      } else {
        console.error('Unexpected response structure:', result);
        throw new Error('Failed to get image data from the API response.');
      }
    } catch (err) {
      console.error('Error generating image:', err);
      setError(`An error occurred: ${err.message}. Please check the console for more details.`);
      displayModal(`An error occurred: ${err.message}. Please try a different prompt or check the console.`);
    } finally {
      setIsLoading(false);
    }
  };

  // Handle Enter key press in the input field
  const handleKeyPress = (event) => {
    if (event.key === 'Enter') {
      generateImage();
    }
  };

  return (
    <div className="min-h-screen bg-gradient-to-br from-slate-900 to-slate-800 text-white flex flex-col items-center justify-center p-4 font-['Inter',_sans-serif]">
      <div className="bg-slate-800 p-8 rounded-xl shadow-2xl w-full max-w-2xl transform transition-all duration-500 hover:scale-105">
        <header className="text-center mb-8">
          <h1 className="text-4xl font-bold text-sky-400">AI Image Generator</h1>
          <p className="text-slate-400 mt-2">Enter a prompt to create a unique image.</p>
        </header>

        {/* Prompt Input Area */}
        <div className="mb-6">
          <label htmlFor="prompt-input" className="block text-sm font-medium text-slate-300 mb-2">
            Image Prompt
          </label>
          <div className="flex flex-col sm:flex-row gap-3">
            <input
              id="prompt-input"
              type="text"
              value={prompt}
              onChange={(e) => setPrompt(e.target.value)}
              onKeyPress={handleKeyPress}
              placeholder="e.g., A futuristic cityscape at sunset"
              className="flex-grow p-4 bg-slate-700 border border-slate-600 rounded-lg focus:ring-2 focus:ring-sky-500 focus:border-sky-500 outline-none transition-colors"
              aria-label="Image prompt input"
            />
            <button
              onClick={generateImage}
              disabled={isLoading}
              className="bg-sky-500 hover:bg-sky-600 text-white font-semibold py-3 px-6 rounded-lg shadow-md hover:shadow-lg transition-all duration-300 ease-in-out disabled:opacity-50 disabled:cursor-not-allowed flex items-center justify-center"
              aria-label="Generate image button"
            >
              {isLoading ? (
                <>
                  <svg className="animate-spin -ml-1 mr-3 h-5 w-5 text-white" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24">
                    <circle className="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" strokeWidth="4"></circle>
                    <path className="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path>
                  </svg>
                  Generating...
                </>
              ) : (
                'Generate Image'
              )}
            </button>
          </div>
        </div>

        {/* Image Display Area */}
        {error && (
          <div className="mt-6 p-4 bg-red-500/20 border border-red-700 text-red-300 rounded-lg text-center" role="alert">
            <p className="font-semibold">Error Generating Image</p>
            <p className="text-sm">{error}</p>
          </div>
        )}

        {isLoading && !imageUrl && (
          <div className="mt-8 text-center">
            <div className="inline-block animate-pulse bg-slate-700 rounded-lg w-full h-64 sm:h-80 md:h-96 flex items-center justify-center">
               <svg className="w-12 h-12 text-slate-500 animate-spin" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24">
                <circle className="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" strokeWidth="4"></circle>
                <path className="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path>
              </svg>
              <p className="ml-3 text-slate-400">Creating your masterpiece...</p>
            </div>
          </div>
        )}

        {imageUrl && !isLoading && (
          <div className="mt-8 border-4 border-sky-500/50 rounded-xl shadow-xl overflow-hidden">
            <img
              src={imageUrl}
              alt="Generated image based on prompt"
              className="w-full h-auto object-contain rounded-lg transition-opacity duration-500 ease-in-out opacity-0 animate-fadeIn"
              style={{ animationFillMode: 'forwards' }}
              onLoad={(e) => e.target.style.opacity = 1}
            />
          </div>
        )}

        {!isLoading && !imageUrl && !error && (
            <div className="mt-8 text-center text-slate-500 py-10 border-2 border-dashed border-slate-700 rounded-lg">
                <svg xmlns="http://www.w3.org/2000/svg" width="48" height="48" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="1.5" strokeLinecap="round" strokeLinejoin="round" className="mx-auto mb-4 opacity-50"><path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4"/><polyline points="17 8 12 3 7 8"/><line x1="12" x2="12" y1="3" y2="15"/></svg>
                <p className="text-lg font-medium">Your generated image will appear here.</p>
                <p className="text-sm">Enter a prompt above and click "Generate Image".</p>
            </div>
        )}
      </div>

      {/* Modal for messages */}
      {showModal && (
        <div className="fixed inset-0 bg-black bg-opacity-75 flex items-center justify-center p-4 z-50 transition-opacity duration-300 ease-in-out">
          <div className="bg-slate-800 p-6 rounded-lg shadow-xl max-w-sm w-full mx-auto border border-sky-500/50">
            <h3 className="text-lg font-semibold text-sky-400 mb-4">Notification</h3>
            <p className="text-slate-300 mb-6">{modalMessage}</p>
            <button
              onClick={() => setShowModal(false)}
              className="w-full bg-sky-500 hover:bg-sky-600 text-white font-medium py-2 px-4 rounded-lg transition-colors"
              aria-label="Close modal button"
            >
              OK
            </button>
          </div>
        </div>
      )}

      {/* CSS for fadeIn animation */}
      <style>{`
        @keyframes fadeIn {
          0% { opacity: 0; transform: scale(0.95); }
          100% { opacity: 1; transform: scale(1); }
        }
        .animate-fadeIn {
          animation: fadeIn 0.5s ease-out forwards;
        }
      `}</style>
    </div>
  );
}
