# Cash Register

<h2>Exercise</h2>
<p>Design a cash register drawer function checkCashRegister() that accepts purchase price as the first argument (price), payment as the second argument (cash), and cash-in-drawer (cid) as the third argument.</p>

<p>cid is a 2D array listing available currency.</p>

<p>The checkCashRegister() function should always return an object with a status key and a change key.</p>

<p>Return {status: "INSUFFICIENT_FUNDS", change: []} if cash-in-drawer is less than the change due, or if you cannot return the exact change.</p>

<p>Return {status: "CLOSED", change: [...]} with cash-in-drawer as the value for the key change if it is equal to the change due.</p>

<p>Otherwise, return {status: "OPEN", change: [...]}, with the change due in coins and bills, sorted in highest to lowest order, as the value of the change key.</p>
    <table>
      <thead>
        <tr>
          <th>Currency Unit</th>
          <th>Amount</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td>Penny</td>
          <td>$0.01 (PENNY)</td>
        </tr>
        <tr>
          <td>Nickel</td>
          <td>$0.05 (NICKEL)</td>
        </tr>
        <tr>
          <td>Dime</td>
          <td>$0.1 (DIME)</td>
        </tr>
        <tr>
          <td>Quarter</td>
          <td>$0.25 (QUARTER)</td>
        </tr>
        <tr>
          <td>Dollar</td>
          <td>$1 (ONE)</td>
        </tr>
        <tr>
          <td>Five Dollars</td>
          <td>$5 (FIVE)</td>
        </tr>
        <tr>
          <td>Ten Dollars</td>
          <td>$10 (TEN)</td>
        </tr>
        <tr>
          <td>Twenty Dollars</td>
          <td>$20 (TWENTY)</td>
        </tr>
        <tr>
          <td>One-hundred Dollars</td>
          <td>$100 (ONE HUNDRED)</td>
        </tr>
      </tbody>
    </table>

See below for an example of a cash-in-drawer array:

```
[
  ["PENNY", 1.01],
  ["NICKEL", 2.05],
  ["DIME", 3.1],
  ["QUARTER", 4.25],
  ["ONE", 90],
  ["FIVE", 55],
  ["TEN", 20],
  ["TWENTY", 60],
  ["ONE HUNDRED", 100]
]
```

<h2>Code</h2>

```
function checkCashRegister(price, cash, cid) {
  const currencyUnit = [
    ["PENNY", 0.01],
    ["NICKEL", 0.05],
    ["DIME", 0.1],
    ["QUARTER", 0.25],
    ["ONE", 1],
    ["FIVE", 5],
    ["TEN", 10],
    ["TWENTY", 20],
    ["ONE HUNDRED", 100]
  ];

  let changeDue = cash - price;
  let changeArray = [];
  let totalCID = 0;

  for (let i = 0; i < cid.length; i++) {
    totalCID += cid[i][1];
  }

  totalCID = parseFloat(totalCID.toFixed(2));

  if (totalCID < changeDue) {
    return { status: "INSUFFICIENT_FUNDS", change: [] };
  } else if (totalCID === changeDue) {
    return { status: "CLOSED", change: cid };
  } else {
    for (let i = currencyUnit.length - 1; i >= 0; i--) {
      const coinName = currencyUnit[i][0];
      const coinValue = currencyUnit[i][1];
      const availableCoins = cid[i][1];

      let numberOfCoins = Math.floor(changeDue / coinValue);
      let returnedAmount = numberOfCoins * coinValue;
      returnedAmount = parseFloat(returnedAmount.toFixed(2));

      if (numberOfCoins > 0 && returnedAmount <= availableCoins) {
        changeArray.push([coinName, returnedAmount]);
        changeDue -= returnedAmount;
        changeDue = parseFloat(changeDue.toFixed(2));
      } else if (numberOfCoins > 0 && returnedAmount > availableCoins) {
        changeArray.push([coinName, availableCoins]);
        changeDue -= availableCoins;
        changeDue = parseFloat(changeDue.toFixed(2));
      }
    }
  }

  if (changeDue > 0) {
    return { status: "INSUFFICIENT_FUNDS", change: [] };
  }

  return { status: "OPEN", change: changeArray };
}
```

<h2>Exemple</h2>

```
checkCashRegister(19.5, 20, [["PENNY", 1.01], ["NICKEL", 2.05], ["DIME", 3.1], ["QUARTER", 4.25], ["ONE", 90], ["FIVE", 55], ["TEN", 20], ["TWENTY", 60], ["ONE HUNDRED", 100]]) 

status: "OPEN", change: [["QUARTER", 0.5]]
```
<a href="https://a-marvulle.github.io/cash/" target=_blank>Pages</a>
