<script>
  import "./main.css";
  import { onMount } from "svelte";

  onMount(() => {
    const initBalance =  document.getElementById('init');
    const initFee =  document.getElementById('fee');
    const finalBalance =  document.getElementById('final');
    const profit =  document.getElementById('profit');
    const profitEach =  document.getElementById('profit-each');
    const debt =  document.getElementById('debt');
    const takeFromAcc =  document.getElementById('take-from-acc');
    const nextInit =  document.getElementById('next-init');
    const nextFee =  document.getElementById('next-fee');
    const nextRealFee =  document.getElementById('next-real-fee');
    const withdrawal =  document.getElementById('withdrawal');
    const reset =  document.getElementById('reset');
    const FEE = 0.07;
    // Magical constant that if multiplied by a numebr, it will result in a number 
    // that equals the equivalent of 7% of that number if reduced from it
    // ex: -> 
    // X some number in R
    // I = (X * EQ)
    // J = X - I
    // J * 7% = I
    const EQ_Const = 0.06542;

    
    function calculate(){
      let diff = Number(finalBalance.value) - Number(initBalance.value);
      let diffInPercent = diff !== 0 ? (Number(diff / initBalance.value) * 100).toFixed(1) : "0";
      profit.value = `${diff} (${diffInPercent}%)`;
      profitEach.value = (diff / 2).toFixed(1);
      initFee.value = (Number(initBalance.value) * FEE).toFixed(1);
      debt.value = (initFee.value - profitEach.value ).toFixed(1);
      if(takeFromAcc.checked){
        let newFinal = (Number(finalBalance.value) + Number(debt.value));
        newFinal = Number(newFinal - (newFinal * EQ_Const)).toFixed(1);
        let withdraw = Math.abs(Number(finalBalance.value) - newFinal);
        nextInit.value = newFinal;
        nextFee.value = (newFinal * FEE).toFixed(1);
        nextRealFee.value = (Number(nextFee.value) - Number(debt.value)).toFixed(1);
        withdrawal.value = Number(withdraw).toFixed(1);
      } else {
        nextInit.value = finalBalance.value;
        nextFee.value = (Number(finalBalance.value) * FEE).toFixed(1);
        nextRealFee.value = (Number(nextFee.value) - Number(debt.value)).toFixed(1);
        withdrawal.value = 0;
      }
    }


    initBalance.onkeyup = calculate;
    finalBalance.onkeyup = calculate;
    takeFromAcc.onchange = calculate;
    reset.onclick = () => {
      initBalance.value = 0;
      finalBalance.value = 0;
      calculate();
    }

  })
</script>



<main>
  <div class="head">
    <h1>HRN Calculator</h1>
    <div class="desc">A Tool to Calculate Profit, Fees, and Debts for <a href="https://www.copytradingwithhrn.com/">HRN Copy Trading Service</a></div>
  </div>

  <div class="container">

    <div class="inputData">
      <div class="item">
        <label for="init">Initial Balance account</label>
        
        <input value="0" type="number" id="init" > $
      </div>
      <div class="item">
        <label for="final">Final Balance</label>
        
        <input value="0" type="number" id="final"> $
      </div>
      <div class="item">
        <label for="take-from-acc">Take Next Month's Fee from Account?</label>
        <input type="checkbox" id="take-from-acc"> 
      </div>
      <div class="item">
        <button id="reset">
          <span class="material-symbols-outlined">
            autorenew
            </span>
            <d>Reset</d>
        </button>
      </div>
    </div>


    <div class="output">

      <div class="item">
        <label for="fee">Starting 7% fee</label>
        
        <input value="0" type="text" id="fee" readonly> $
      </div>
  
      <div class="item">
        <label for="profit-each">Profit</label>
        
        <input value="0"  id="profit" readonly style="max-width: 200px;"> $
      </div>
  
      <div class="item">
        <label for="profit-each">Profit for each</label>
        
        <input value="0"  id="profit-each" readonly> $
      </div>
  
      <div class="item">
        <label for="debt">Debt</label>
        
        <input value="0"  id="debt" readonly> $
        <p>hint: negative number means Client should pay the debt</p>
      </div>
  
  
      <div class="item">
        <label for="next-init">Next month's Starting Balance</label>
        
        <input value="0"  id="next-init" readonly> $
      </div>
  
      <div class="item">
        <label for="next-fee">Next month's 7% fee </label>
        
        <input value="0"  id="next-fee" readonly> $
      </div>
  
      <div class="item">
        <label for="next-real-fee">Next month's Net fee (realized with debt)</label>
        
        <input value="0"  id="next-real-fee" readonly> $
      </div>
    
      <div class="item">
        <label for="withdrawal">Withdrawal</label>
        
        <input value="0"  id="withdrawal" readonly> $
      </div>
  
    </div>

  </div>




</main>
