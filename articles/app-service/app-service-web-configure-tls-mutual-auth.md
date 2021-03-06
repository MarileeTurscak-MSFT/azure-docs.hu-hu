---
title: TLS kölcsönös hitelesítés beállítása webalkalmazáshoz
description: Ismerje meg, hogyan konfigurálja a webappot a TLS ügyfél Tanúsítványalapú hitelesítés használatára.
services: app-service
documentationcenter: ''
author: naziml
manager: erikre
editor: jimbe
ms.assetid: cd1d15d3-2d9e-4502-9f11-a306dac4453a
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/08/2016
ms.author: naziml
ms.openlocfilehash: 894a77be05de131ab122f18c62d209e9829357f9
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/14/2018
ms.locfileid: "39056208"
---
# <a name="how-to-configure-tls-mutual-authentication-for-web-app"></a>TLS kölcsönös hitelesítés beállítása webalkalmazáshoz
## <a name="overview"></a>Áttekintés
Hozzáférés az Azure-webalkalmazást, hitelesítési típust engedélyezésével korlátozhatja. Ennek egyik módja a hitelesítés ügyféltanúsítvány használatával, ha a rendszer a TLS/SSL-en keresztül. Ez a mechanizmus TLS kölcsönös hitelesítés vagy a hitelesítést, és ez a cikk részletesen ügyféltanúsítvány-alapú hitelesítés használatára a webalkalmazás beállítása ügyféltanúsítvány nevezzük.

> **Megjegyzés:** a webhely a HTTP és HTTPS-nem keresztül éri el, ha bármely ügyfél-tanúsítvány nem fog kapni. Tehát ha az alkalmazás ügyfél-tanúsítványok nem engedélyezze kérelmeket az alkalmazás HTTP-n keresztül.
> 
> 

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="configure-web-app-for-client-certificate-authentication"></a>Ügyféltanúsítvány-alapú hitelesítés a webalkalmazás konfigurálása
A webalkalmazás beállítása az ügyféltanúsítványok megköveteléséhez, kell adja hozzá a webalkalmazás az ügyféltanúsítvány engedélyezésével hely beállítást, és állítsa igaz értékre. A beállítás akkor is konfigurálhatók, az SSL-tanúsítványok panel alatt az Azure Portalon.

Használhatja a [ARMClient eszköz](https://github.com/projectkudu/ARMClient) megkönnyíti a REST API-hívás írhat. Miután jelentkezik be az eszközt, szüksége lesz az alábbi parancsot:

    ARMClient PUT subscriptions/{Subscription Id}/resourcegroups/{Resource Group Name}/providers/Microsoft.Web/sites/{Website Name}?api-version=2015-04-01 @enableclientcert.json -verbose

cserélje le a tartalmát {} a adatokkal, a webes alkalmazás, és hozzon létre egy fájlt nevű enableclientcert.json a következő JSON-tartalom:

    {
        "location": "My Web App Location",
        "properties": {
            "clientCertEnabled": true
        }
    }

Ellenőrizze, hogy a "hely" értékét módosítsa arra, bárhol is legyenek a webalkalmazás található példa, USA északi középső Régiója és USA nyugati RÉGIÓJA stb.

Is https://resources.azure.com tükrözés, a `clientCertEnabled` tulajdonságot `true`.

> **Megjegyzés:** ARMClient powershellből futtatásakor, szüksége lesz karakterpárt a \@ a JSON-fájlt a háttérrendszer osztásjelek szimbólum ".
> 
> 

## <a name="accessing-the-client-certificate-from-your-web-app"></a>Az ügyféltanúsítvány a webes alkalmazás elérése
Használja az ASP.NET, és állítsa be alkalmazását az ügyféltanúsítvány-alapú hitelesítés használatára, ha a tanúsítvány érhető el a **HttpRequest.ClientCertificate** tulajdonság. Más alkalmazáscsoportokat az alkalmazásban a "X-ARR-ClientCert" kérés fejlécében base64-kódolású értéket keresztül elérhető lesz az ügyféltanúsítvány. Az alkalmazás is hozzon létre egy tanúsítványt a ezt az értéket, majd az alkalmazás hitelesítési és engedélyezési célból.

## <a name="special-considerations-for-certificate-validation"></a>Különleges szempontok a tanúsítvány érvényesítése
Az ügyféltanúsítvány, amelyet az alkalmazás elküld nem halad át minden érvényesítése az Azure Web Apps platformon. Ez a tanúsítvány érvényesítése feladata a webalkalmazás. Itt látható minta ASP.NET-kód, amely ellenőrzi a hitelesítési tanúsítvány tulajdonságai.

    using System;
    using System.Collections.Specialized;
    using System.Security.Cryptography.X509Certificates;
    using System.Web;

    namespace ClientCertificateUsageSample
    {
        public partial class cert : System.Web.UI.Page
        {
            public string certHeader = "";
            public string errorString = "";
            private X509Certificate2 certificate = null;
            public string certThumbprint = "";
            public string certSubject = "";
            public string certIssuer = "";
            public string certSignatureAlg = "";
            public string certIssueDate = "";
            public string certExpiryDate = "";
            public bool isValidCert = false;

            //
            // Read the certificate from the header into an X509Certificate2 object
            // Display properties of the certificate on the page
            //
            protected void Page_Load(object sender, EventArgs e)
            {
                NameValueCollection headers = base.Request.Headers;
                certHeader = headers["X-ARR-ClientCert"];
                if (!String.IsNullOrEmpty(certHeader))
                {
                    try
                    {
                        byte[] clientCertBytes = Convert.FromBase64String(certHeader);
                        certificate = new X509Certificate2(clientCertBytes);
                        certSubject = certificate.Subject;
                        certIssuer = certificate.Issuer;
                        certThumbprint = certificate.Thumbprint;
                        certSignatureAlg = certificate.SignatureAlgorithm.FriendlyName;
                        certIssueDate = certificate.NotBefore.ToShortDateString() + " " + certificate.NotBefore.ToShortTimeString();
                        certExpiryDate = certificate.NotAfter.ToShortDateString() + " " + certificate.NotAfter.ToShortTimeString();
                    }
                    catch (Exception ex)
                    {
                        errorString = ex.ToString();
                    }
                    finally 
                    {
                        isValidCert = IsValidClientCertificate();
                        if (!isValidCert) Response.StatusCode = 403;
                        else Response.StatusCode = 200;
                    }
                }
                else
                {
                    certHeader = "";
                }
            }

            //
            // This is a SAMPLE verification routine. Depending on your application logic and security requirements, 
            // you should modify this method
            //
            private bool IsValidClientCertificate()
            {
                // In this example we will only accept the certificate as a valid certificate if all the conditions below are met:
                // 1. The certificate is not expired and is active for the current time on server.
                // 2. The subject name of the certificate has the common name nildevecc
                // 3. The issuer name of the certificate has the common name nildevecc and organization name Microsoft Corp
                // 4. The thumbprint of the certificate is 30757A2E831977D8BD9C8496E4C99AB26CB9622B
                //
                // This example does NOT test that this certificate is chained to a Trusted Root Authority (or revoked) on the server 
                // and it allows for self signed certificates
                //

                if (certificate == null || !String.IsNullOrEmpty(errorString)) return false;

                // 1. Check time validity of certificate
                if (DateTime.Compare(DateTime.Now, certificate.NotBefore) < 0 || DateTime.Compare(DateTime.Now, certificate.NotAfter) > 0) return false;

                // 2. Check subject name of certificate
                bool foundSubject = false;
                string[] certSubjectData = certificate.Subject.Split(new char[] { ',' }, StringSplitOptions.RemoveEmptyEntries);
                foreach (string s in certSubjectData)
                {
                    if (String.Compare(s.Trim(), "CN=nildevecc") == 0)
                    {
                        foundSubject = true;
                        break;
                    }
                }
                if (!foundSubject) return false;

                // 3. Check issuer name of certificate
                bool foundIssuerCN = false, foundIssuerO = false;
                string[] certIssuerData = certificate.Issuer.Split(new char[] { ',' }, StringSplitOptions.RemoveEmptyEntries);
                foreach (string s in certIssuerData)
                {
                    if (String.Compare(s.Trim(), "CN=nildevecc") == 0)
                    {
                        foundIssuerCN = true;
                        if (foundIssuerO) break;
                    }

                    if (String.Compare(s.Trim(), "O=Microsoft Corp") == 0)
                    {
                        foundIssuerO = true;
                        if (foundIssuerCN) break;
                    }
                }

                if (!foundIssuerCN || !foundIssuerO) return false;

                // 4. Check thumprint of certificate
                if (String.Compare(certificate.Thumbprint.Trim().ToUpper(), "30757A2E831977D8BD9C8496E4C99AB26CB9622B") != 0) return false;

                // If you also want to test if the certificate chains to a Trusted Root Authority you can uncomment the code below
                //
                //X509Chain certChain = new X509Chain();
                //certChain.Build(certificate);
                //bool isValidCertChain = true;
                //foreach (X509ChainElement chElement in certChain.ChainElements)
                //{
                //    if (!chElement.Certificate.Verify())
                //    {
                //        isValidCertChain = false;
                //        break;
                //    }
                //}
                //if (!isValidCertChain) return false;

                return true;
            }
        }
    }
