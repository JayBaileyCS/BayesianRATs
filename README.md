<!DOCTYPE html>
<html>
  <head>
    <style>
      table {
        font-family: arial, sans-serif;
        border-collapse: collapse;
        width: 400px;
      }

      td,
      th {
        border: 1px solid #dddddd;
        text-align: left;
        padding: 8px;
      }

      tr:nth-child(even) {
        background-color: #dddddd;
      }
    </style>
  </head>
  <body>
    <h2>COVID-19 Rapid Antigen Test Probabilities</h2>

    <p>
      <b>Specificity</b> is how likely a test is to return a positive result if
      you have COVID. By default, this is set to 80%, the minimum required for
      an RAT to be sold.
    </p>

    <p>
      <b>Sensitivity</b> is how likely a test is to return a negative result if
      you don't have COVID. By default, this is set to 98%, the minimum required
      for an RAT to be sold.
    </p>

    <p>
      <b>Probability</b> is how likely you are to have COVID before the test.
      This should be the percentage of people in your area who have COVID if you have no symptoms. If you do have symptoms, increase the probability accordingly. I don't have the maths yet.
    </p>

    <table>
      <tr>
        <th>Variable</th>
        <th>Number</th>
      </tr>
      <tr>
        <td><b>Specificity</b> (0-100%)</td>
        <td>
          <input type="text" id="specificity" name="specificity" value="80" />
          <td></td>
        </td>
      </tr>

      <tr>
        <td><b>Sensitivity</b> (0-100%)</td>
        <td>
          <input type="text" id="sensitivity" name="sensitivity" value="98" />
          <td></td>
        </td>
      </tr>

      <tr>
        <td><b>Probability</b> (0-100%)</td>
        <td>
          <input type="text" id="probability" name="probability" value="1" />
          <td></td>
        </td>
      </tr>
    </table>
    <button onclick=getCalculation()>Calculate</button>
    <p id=calculationText></p>

    <script>
        const getCalculation = () => {
          let specificity = Number(document.getElementById("specificity").value) / 100;
          let sensitivity = Number(document.getElementById("sensitivity").value) / 100;
          let probability = Number(document.getElementById("probability").value) / 100;
  
          let true_positive = specificity * probability
          let false_positive = (1 - specificity) * probability
          let true_negative = sensitivity * (1 - probability)
          let false_negative = (1 - sensitivity) * (1 - probability)
  
          let positive_chance = true_positive / (true_positive + false_negative) * 100
          let negative_chance = false_positive / (false_positive + true_negative) * 100

          positive_text = `If you test positive for COVID-19, the chance you actually have it is ${positive_chance.toFixed(2)}%.`
          negative_text = `If you test negative for COVID-19, the chance you actually have it is ${negative_chance.toFixed(2)}%.`

          details_header = `Details:`

          tp_text = `True Positive: The chance you have COVID-19 and test positive is ${(true_positive * 100).toFixed(2)}%.`
          fp_text = `False Positive: The chance you have COVID-19 but test negative is ${(false_positive * 100).toFixed(2)}%.`
          tn_text = `True Negative: The chance you don't have COVID-19 and test negative is ${(true_negative * 100).toFixed(2)}%.`
          fn_text = `False Negative: The chance you don't have COVID-19 but test positive is ${(false_negative * 100).toFixed(2)}%.`

          text_html = `${positive_text}<br>${negative_text}<br><br>${details_header}<br><br>${tp_text}<br>${fp_text}<br>${tn_text}<br>${fn_text}`
  
          document.getElementById("calculationText").innerHTML = text_html;
        }
    </script>
  </body>
</html>
