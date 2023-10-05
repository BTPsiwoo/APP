   @Library('piper-lib-os@v1.53.0') _


create-services:
- name:   "abapEnvironmentPipeline"
  broker: "abap"
  plan:   "16_abap_64_db"
  parameters: "{ \"admin_email\" : \"user@example.com\", \"description\" : \"System for ABAP Pipeline\" }"

  {
  "scenario_id": "SAP_COM_0510",
  "type": "basic"
}

atcobjects:
  softwarecomponent:
    - name: "/DMO/REPO"

    general:
  cfApiEndpoint: 'https://api.cf.sap.hana.ondemand.com'
  cfOrg: 'your-cf-org'
  cfSpace: 'yourSpace'
  cfCredentialsId: 'cfAuthentification'
  cfServiceInstance: 'abapEnvironmentPipeline'
  cfServiceKeyName: 'jenkins_sap_com_0510'
stages:
  Prepare System:
    cfServiceManifest: 'manifest.yml'
    cfServiceKeyConfig: 'sap_com_0510.json'
  Clone Repositories:
    repositoryNames: ['/DMO/REPO']
  ATC:
    atcConfig: 'atcConfig.yml'
steps:
  cloudFoundryDeleteService:
    cfDeleteServiceKeys: true

    H H(3-4) * * *

    void call(Map params) {
  //access stage name
  echo "Start - Extension for stage: ${params.stageName}"

  //access config
  echo "Current stage config: ${params.config}"

  //execute original stage as defined in the template
  params.originalStage()

  recordIssues tools: [checkStyle(pattern: '**/ATCResults.xml')], qualityGates: [[threshold: 1, type: 'TOTAL', unstable: true]]

  echo "End - Extension for stage: ${params.stageName}"
}
return this