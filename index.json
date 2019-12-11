// See https://github.com/dialogflow/dialogflow-fulfillment-nodejs
// for Dialogflow fulfillment library docs, samples, and to report issues
'use strict';

const axios = require('axios');
 
const functions = require('firebase-functions');
const {WebhookClient} = require('dialogflow-fulfillment');
const {Card, Suggestion} = require('dialogflow-fulfillment');
 
process.env.DEBUG = 'dialogflow:debug'; // enables lib debugging statements
 
exports.dialogflowFirebaseFulfillment = functions.https.onRequest((request, response) => {
  const agent = new WebhookClient({ request, response });
  console.log('Dialogflow Request headers: ' + JSON.stringify(request.headers));
  console.log('Dialogflow Request body: ' + JSON.stringify(request.body));
 
  function welcome(agent) {
    agent.add(`Welcome to my agent!`);
  }
 
  function fallback(agent) {
    agent.add(`I didn't understand`);
    agent.add(`Please speak for moving chess piece like "Move A3 B4".`);
  }
  
  function callAPI (agent){
    
    var passCommand = agent.parameters.MoveEntity;
    var passStartposition = agent.parameters.StartPositionEntity;
    var passEndposition = agent.parameters.EndPositionEntity;
   	
    // Senitizing parameters to common format Command StartPosition EndPostion For Ex: Move A1 B2
	if (!passCommand ) {
    	passCommand = 'Move';
    }
    passCommand = passCommand.toUpperCase();
    
    // Removing spaces and changing number written in word to digit
    passStartposition = passStartposition.replace(/ /gi, "");
    passStartposition = passStartposition.toUpperCase();
    passStartposition = passStartposition.replace(/one/gi, "1");
    passStartposition = passStartposition.replace(/two/gi, "2");
    passStartposition = passStartposition.replace(/three/gi, "3");
    passStartposition = passStartposition.replace(/four/gi, "4");
    passStartposition = passStartposition.replace(/five/gi, "5");
    passStartposition = passStartposition.replace(/six/gi, "6");
    passStartposition = passStartposition.replace(/seven/gi, "7");
    passStartposition = passStartposition.replace(/eight/gi, "8");
    
    // Removing spaces and changing number written in word to digit
    passEndposition = passEndposition.replace(/ /gi, "");
    passEndposition = passEndposition.toUpperCase();
    passEndposition = passEndposition.replace(/one/gi, "1");
    passEndposition = passEndposition.replace(/two/gi, "2");
    passEndposition = passEndposition.replace(/three/gi, "3");
    passEndposition = passEndposition.replace(/four/gi, "4");
    passEndposition = passEndposition.replace(/five/gi, "5");
    passEndposition = passEndposition.replace(/six/gi, "6");
    passEndposition = passEndposition.replace(/seven/gi, "7");
    passEndposition = passEndposition.replace(/eight/gi, "8");
    
    const spassCommand = passCommand;
    const spassStartposition = passStartposition;
    const spassEndposition = passEndposition;
    
    //agent.add('Response from API');
    agent.add('API Called with parameters: ' + spassCommand + " , " + spassStartposition + " , " + spassEndposition);

    return axios.get(`https://my-json-server.typicode.com/hsaxena-vub/FakeChessBoardResponse/instruction?command=${spassCommand}&startposition=${spassStartposition}&endposition=${spassEndposition}`)
    .then((result) => {
      result.data.map (responsedata => {
        agent.add('Response from API ' + responsedata.responseofcommand);
      });
            
    });
    
      }

  // Run the proper function handler based on the matched Dialogflow intent name
  let intentMap = new Map();
  intentMap.set('Default Welcome Intent', welcome);
  intentMap.set('Default Fallback Intent', fallback);
  // intentMap.set('your intent name here', yourFunctionHandler);
  intentMap.set('Test', callAPI);
  agent.handleRequest(intentMap);
});
