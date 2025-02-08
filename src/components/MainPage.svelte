<script lang="ts">
    const originalText = "welcome to the party";
    const characters = originalText.split('');
    const TOTAL_DURATION = 1000;
    const PAUSE_BETWEEN_PHASES = 1000;
    const BASE_DELAY = TOTAL_DURATION / characters.length; 
    
    let displayedText: string = $state("");
    let currentAddIndex: number = $state(0);
    let currentRemoveIndex: number = $state(0);
    
    function easeOutBounce(x: number): number {
      const n1 = 7.5625;
      const d1 = 2.75;
      
      if (x < 1 / d1) return n1 * x * x;
      else if (x < 2 / d1) return n1 * (x -= 1.5 / d1) * x + 0.75;
      else if (x < 2.5 / d1) return n1 * (x -= 2.25 / d1) * x + 0.9375;
      else return n1 * (x -= 2.625 / d1) * x + 0.984375;
    }
    
    function easeInBounce(x: number): number {
      return 1 - easeOutBounce(1 - x);
    }
    
    // Phase 1: Add characters
    function addCharacters() {
      if (currentAddIndex < characters.length) {
        displayedText += characters[currentAddIndex];
        currentAddIndex++;
        
        const progress = currentAddIndex / characters.length;
        const delay = BASE_DELAY * (1 + easeInBounce(progress));
        
        setTimeout(addCharacters, delay); // Fixed function name typo
      } else {
        setTimeout(startRemovalPhase, PAUSE_BETWEEN_PHASES); // Fixed constant name
      }
    }
    
    // Phase 2: Remove characters
    function startRemovalPhase() {
      removeCharacter();
    }
    
    function removeCharacter() {
      if (displayedText.length > 0) {
        displayedText = displayedText.slice(0, -1);
        currentRemoveIndex++;
        
        const progress = currentRemoveIndex / characters.length;
        const delay = BASE_DELAY * (1 + easeOutBounce(progress));
        
        setTimeout(removeCharacter, delay);
      }
    }
    
    addCharacters();
    </script>
    
    <span id="helloText">
      {displayedText} 
    </span>