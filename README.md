# JioMoney_JAVA_Web_Sample_Kit

This SDK (Jiomoney-java-sdk.jar) will provide the below listed APIs.
1.	PURCHASE
2.	REFUND
3.	CHECKPAYMENTSTATUS
4.	GETREQUESTSTATUS
5.	GETMDR
6.	GETTRANSACTIONDETAILS
7.	FETCHTRANSACTIONPERIOD
8.	GETTODAYSDATA
9.	STATUSQUERY

# Integration Steps

Copy the jiomoney-java-sdk.jar file and paste in project’s lib folder.</br>
Copy the gson-2.2.4.jar (Version may be differ) file and paste in project’s lib folder.</br>
One time step, Set the JioMoney Configuration. Call the JioMoneyConfig constructor and set all the below mentioned parameters of constructor.
1. Environment (Environment is the enum class of the SDK)
2. Client Id (Provided by the JioMoney)
3. Merchant Id (Provided by the JioMoney)
4. Checksum Seed (Provided by the JioMoney)



Sample Code:</br>
new JioMoneyConfig(Environment.PRE_PROD, Your Client Id, Your Merchant Id, Your Checksum Seed);

# PURCHASE API Steps and sample code</br>
Step 1. Create Java Servlet</br>
Step 2. In the doGet method of servlet you need prepare the request and call the Purchase method</br>
Step 3. Set the WebServlet for your servlet url to run</br>
Step 4. Run your servlet</br>

Below is the complete sample code to call the PURCHASE API.

@WebServlet("/testPurchase")</br>
public class Test extends HttpServlet {</br>
	private static final long serialVersionUID = 1L;

	public Test() {
		super();
	}

protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

		PurchaseRequest purchaseRequest = new PurchaseRequest();
		purchaseRequest.setVersion("2.0");
		purchaseRequest.setMerchantname("test name");
		purchaseRequest.setChannel("WEB");
		purchaseRequest.setReturl("Your return url");
		purchaseRequest.setExtref(JioMoneyUtil.getTranRefNumber());
		purchaseRequest.setTimestamp(JioMoneyUtil.getCurrentTimestamp());
		purchaseRequest.setAmount("1.00");
		purchaseRequest.setCurrency("INR");
		purchaseRequest.setMobilenumber("9619226473");
		purchaseRequest.setCustomerid("1000523403");
		purchaseRequest.setProductdescription("testproduct");
		purchaseRequest.setUdf1("ud11");
		purchaseRequest.setUdf2("ud22");
		purchaseRequest.setUdf3("ud33");
		purchaseRequest.setUdf4("ud44");
		purchaseRequest.setUdf5("ud55");

		response.getWriter().println(PurchaseService.getPurchaseRequest(purchaseRequest));
		response.getWriter().close();
	}
}

Note: PURCHASE response will come to your return URL.

# REFUND API Steps and sample code</br>
Step 1. Prepare the request model</br>
Step 2. Call the actual API method, This method will return the JSON String Response</br>
Step 3. Parse the JSON String Response, This method will return you the Response Object </br>
Note: Before parse you need to check the String response, if the accepted response return in the Step 3 than you can call parse the response method.</br>

Complete Sample Code for the REFUND API:</br>
 
public static void refund() {</br>
		
		RefundRequestHeader requestHeader = new RefundRequestHeader();
		requestHeader.setVersion("2.0");
		requestHeader.setTimestamp(JioMoneyUtil.getCurrentTimestamp());
		RefundRequestPayloadData payloadData = new RefundRequestPayloadData();
		payloadData.setAdditional_info("NA");
		payloadData.setMobile_no("9619226473");
		payloadData.setOrg_jm_tran_ref_no("901033485327");
		payloadData.setOrg_txn_timestamp("20170906151601");
		payloadData.setTran_ref_no(JioMoneyUtil.getTranRefNumber());
		payloadData.setTxn_amount("1.00");

		RefundRequest request = new RefundRequest();
		request.setRequest_header(requestHeader);
		request.setPayload_data(payloadData);

		RefundRequestMain refundRequestMain = new RefundRequestMain();
		refundRequestMain.setRequest(request);
		String stringResponse = RefundService.refund(refundRequestMain);

		RefundResponseMain refundResponse = JioMoneyJSONParser.parseRefundResponse(stringResponse);
	}


# CHECKPAYMENTSTATUS API Steps and sample code
Step 1. Prepare the request model</br>
Step 2. Call the actual API method, This method will return the JSON String Response</br>
Step 3. Parse the JSON String Response, This method will return you the Response Object. 
Note: Before parse you need to check the String response, if the accepted response return in the Step 3 than you can call parse the response method.</br>

Complete Sample Code for the CHECKPAYMENTSTATUS API:</br>

public static void checkPaymentStatus() {

		CheckPaymentStatusRequestHeader requestHeader = new CheckPaymentStatusRequestHeader();
		requestHeader.setRequest_id(JioMoneyUtil.getRequestId());
		requestHeader.setTimestamp(JioMoneyUtil.getCurrentTimestamp());

		ArrayList<String> listOfTranRefNo = new ArrayList<>();
		listOfTranRefNo.add("4F32N6JCFM5J");
		CheckPaymentStatusRequestTranDetailsBean tranDetails = new CheckPaymentStatusRequestTranDetailsBean();
		tranDetails.setTran_ref_no(listOfTranRefNo);

		CheckPaymentStatusRequestPayloadData payloadData = new CheckPaymentStatusRequestPayloadData();
		payloadData.setTran_details(tranDetails);
		payloadData.setTxntimestamp("20170906151601");

		CheckPaymentStatusRequest request = new CheckPaymentStatusRequest();
		request.setRequest_header(requestHeader);
		request.setPayload_data(payloadData);

		CheckPaymentStatusRequestMain checkPaymentStatusRequest = new CheckPaymentStatusRequestMain();
		checkPaymentStatusRequest.setRequest(request);
		String response = CheckPaymentStatusService.checkPaymentStatus(checkPaymentStatusRequest);

		CheckPaymentStatusResponseMain checkPaymentStatusResponse = JioMoneyJSONParser
				.parseCheckPaymentStatusResponse(response);
	}
	 
# Other APIs Steps and sample code

Step 1. Prepare the request model</br>
Step 2. Call the actual API method, This method will return the JSON String Response</br>
Step 3. Parse the JSON String Response, This method will return you the Response Object. </br>
Note: Before parse you need to check the String response, if the accepted response return in the Step 3 than you can call parse the response method.</br> 

Complete Sample Code for the GETREQUESTSTATUS API:

public static void getRequestStatus() {

		GetRequestStatusRequest getRequestStatusRequest = new GetRequestStatusRequest();
		getRequestStatusRequest.setMODE("2");
		getRequestStatusRequest.setREQUESTID(JioMoneyUtil.getRequestId());
		getRequestStatusRequest.setTRANID("901033485327");
		String response = GetRequestStatusService.getRequestStatus(getRequestStatusRequest);
		GetRequestStatusResponseMain getRequestStatusResponse = JioMoneyJSONParser
				.parseGetRequestStatusResponse(response);
	}


Complete Sample Code for the GETMDR API:

public static void getMDR() {

		GetMDRRequest getMDRRequest = new GetMDRRequest();
		getMDRRequest.setMODE("2");
		getMDRRequest.setREQUESTID(JioMoneyUtil.getRequestId());
		getMDRRequest.setTRANID("901033485327");
		String response = GetMDRService.getMDR(getMDRRequest);
		GetMDRResponseMain mdrResponse = JioMoneyJSONParser.parseGetMDRResponse(response);
}


Complete Sample Code for the GETTRANSACTIONDETAILS API:

public static void getTransactionDetails() {

		GetTransactionDetailsRequest getTransactionDetailsRequest = new GetTransactionDetailsRequest();
		getTransactionDetailsRequest.setMODE("2");
		getTransactionDetailsRequest.setREQUESTID(JioMoneyUtil.getRequestId());
		getTransactionDetailsRequest.setTRANID("901033485327");
		String response = GetTransactionDetailsService.getTransactionDetails(getTransactionDetailsRequest);
		GetTransactionDetailsResponseMain getTransactionDetailsResponse = JioMoneyJSONParser
				.parseGetTransactionDetailsResponse(response);
	}


Complete Sample Code for the FETCHTRANSACTIONPERIOD API:

public static void fetchTransactionPeriod() {
		
		FetchTransactionPeriodRequest fetchTransactionPeriodRequest = new FetchTransactionPeriodRequest();
		fetchTransactionPeriodRequest.setMODE("2");
		fetchTransactionPeriodRequest.setREQUESTID(JioMoneyUtil.getRequestId());
		fetchTransactionPeriodRequest.setTRANID("901033485327");
		fetchTransactionPeriodRequest.setSTARTDATETIME("");
		fetchTransactionPeriodRequest.setENDDATETIME("");
		String response = FetchTransactionPeriodService.fetchTransactionPeriod(fetchTransactionPeriodRequest);
		FetchTransactionPeriodResponseMain fetchTransactionPeriodResponse = JioMoneyJSONParser
				.parseFetchTransactionPeriodResponse(response);
	}


Complete Sample Code for the GETTODAYSDATA API:

public static void getTodaysData() {
		
		GetTodaysDataRequest getTodaysDataRequest = new GetTodaysDataRequest();
		getTodaysDataRequest.setMODE("2");
		getTodaysDataRequest.setREQUESTID(JioMoneyUtil.getRequestId());
		getTodaysDataRequest.setTRANID("901033485327");
		String response = GetTodaysDataService.getTodaysData(getTodaysDataRequest);
		GetTodaysDataResponseMain todaysDataResponse = JioMoneyJSONParser.parseGetTodaysDataResponse(response);
	}
	
	
Complete Sample Code for the STATUSQUERY API:

public static void statusQuery() {

		StatusQueryRequestHeader request_header = new StatusQueryRequestHeader();
		request_header.setVersion("1.0");

		StatusQueryRequestPayloadData payload_data = new StatusQueryRequestPayloadData();
		payload_data.setTran_ref_no("4F32N6JCFM5J");

		StatusQueryRequest statusQueryRequest = new StatusQueryRequest();
		statusQueryRequest.setRequest_header(request_header);
		statusQueryRequest.setPayload_data(payload_data);
		String response = StatusQueryService.statusQuery(statusQueryRequest);
		StatusQueryResponse statusQueryResponse = JioMoneyJSONParser.parseStatusQueryResponse(response);
	}
