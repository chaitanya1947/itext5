package com.google.FileTesting;
import java.io.FileInputStream;
//import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.text.SimpleDateFormat;
//import java.util.Calendar;
import java.util.Properties;

import com.itextpdf.text.Chunk;
import com.itextpdf.text.Document;
import com.itextpdf.text.DocumentException;
import com.itextpdf.text.Element;
import com.itextpdf.text.Font;
import com.itextpdf.text.List;
import com.itextpdf.text.ListItem;
import com.itextpdf.text.Paragraph;
import com.itextpdf.text.Phrase;
import com.itextpdf.text.pdf.PdfPCell;
import com.itextpdf.text.pdf.PdfPTable;
import com.itextpdf.text.pdf.PdfWriter;
import com.itextpdf.text.pdf.draw.VerticalPositionMark;

public class Pdf {

	//public static void main(String[] args) throws DocumentException, IOException {
	@SuppressWarnings("unused")
	public String pdfCreation(String path,TWObject Object,TWObject Object1,String destinationPath) throws IOException, DocumentException {

		//String path = "C:\\Users\\alekhyac\\eclipse-workspace\\Doc\\config.properties";

		//int  TIME = Calendar.getInstance().get(Calendar.SECOND);

		Document document = new Document();
		String candidateName=(String) Object.getPropertyValue("employeeName");
        //System.out.println(candidateName);
        //String newFileName="OfferLetter_" + candidateName.replaceAll(" ", "_") + ".docx";
        String pdfFileName="OfferLetter_" + candidateName; 
		PdfWriter pdfWriter = PdfWriter.getInstance(document, new FileOutputStream(destinationPath +pdfFileName + ".pdf"));
				
		document.open();

		Properties prop = new Properties();
		//System.out.println("dgfd");
		InputStream input =new FileInputStream(path);
		//System.out.println("dgfd");
		prop.load(input);

		Paragraph paragraph1 = new Paragraph();
		paragraph1.add(new Chunk(prop.getProperty("db.p0")));

		paragraph1.setAlignment(Element.ALIGN_CENTER);
		paragraph1.setSpacingAfter(50);
		document.add(paragraph1);

		// ----------------------------
		Font fontcabecatable = new Font(Font.FontFamily.TIMES_ROMAN, 13, Font.BOLD);

		Font timesRomanfont = new Font(Font.FontFamily.TIMES_ROMAN, 13, Font.UNDERLINE | Font.BOLD);

		Chunk timesRomanChunk = new Chunk("Letter of Intent", timesRomanfont);

		Paragraph paragraph2 = new Paragraph();
		
		paragraph2.add(timesRomanChunk);
		paragraph2.setAlignment(Element.ALIGN_CENTER);

		document.add(paragraph2);

		// ------------------------------
		SimpleDateFormat sdf=new SimpleDateFormat("dd MMMM yyyy");
		
		Phrase phraseA11 = new Phrase();
		Chunk glue= new Chunk(new VerticalPositionMark());

		phraseA11.add("To,");
		phraseA11.add(new Chunk(glue));
		phraseA11.add(/*"OfferReleaseDate"*/sdf.format(((java.util.GregorianCalendar)Object.getPropertyValue("offerReleaseDate")).getTime()));

		Paragraph paragraphA11 = new Paragraph();
		paragraphA11.add(phraseA11);
		paragraphA11.setFont(fontcabecatable);
		
		document.add(new Phrase("\n"));

		document.add(paragraphA11);			
		

		// ------------------------------------

		Phrase phrase = new Phrase();

		phrase.add(/*"Mr.NAME "*/(String) Object.getPropertyValue("employeeName"));
		phrase.add(Chunk.NEWLINE);
		phrase.add(/*"Address"*/(String) Object.getPropertyValue("employeeAddress"));
		phrase.add(new Phrase("\n"));
		


		document.add(Chunk.NEWLINE);

		document.add(phrase);
		
		document.add(Chunk.NEWLINE);
		document.add(new Chunk("Dear "+ (String) Object.getPropertyValue("employeeName")+" ,"));

		// ------------------
		document.add(Chunk.NEWLINE);
		String property1 = prop.getProperty("db.p1");

		Paragraph paragraph3 = new Paragraph();

		Chunk chunk3 = new Chunk(property1);

		paragraph3.add(chunk3);
		paragraph3.setAlignment(Element.ALIGN_JUSTIFIED);
		
		document.add(new Phrase("\n"));
		document.add(paragraph3);
		document.add(Chunk.NEWLINE);

		// -------------------------------
		String property2 = prop.getProperty("db.p2");

		Paragraph paragraph4 = new Paragraph();
		Chunk chunk4 = new Chunk(property2);
		paragraph4.add(chunk4);
		paragraph4.setAlignment(Element.ALIGN_MIDDLE);
		paragraph4.add(new Phrase(/*"DESIGNATION"*/" \" "+(String) Object.getPropertyValue("designation")+" \" "));
		paragraph4.add(new Phrase(" on the following terms and conditions"));

		document.add(paragraph4);
		document.add(Chunk.NEWLINE);
		// -------------------------

		Chunk chunk5 = new Chunk("Compensation & Benefits", timesRomanfont);
		Paragraph paragraph5 = new Paragraph();
		paragraph5.add(chunk5);

		document.add(paragraph5);
		document.add(Chunk.NEWLINE);
		// ----------------------------------

		String property3 = prop.getProperty("db.p3");
		String property4 = prop.getProperty("db.p4");

//		Chunk chunk6 = new Chunk(property3);
		
		Paragraph paragraph14 = new Paragraph();
	
		Phrase phrase2 = new Phrase();
		phrase2.add(property3);
		phrase2.add(/*"Rs.CTC"*/(String) Object.getPropertyValue("ctc"));
		
		phrase2.add(" per annum.");
		paragraph14.add(phrase2);
		
		document.add(paragraph14);
		document.add(Chunk.NEWLINE);
		document.add(new Chunk(property4));
		document.add(Chunk.NEWLINE);
		document.add(Chunk.NEWLINE);

		//document.add(new Chunk("BONUS"));
       	String bonus=((String)Object.getPropertyValue("bonusAmount"));
       	//System.out.println("Bonus : '" + bonus + "'");
      
       	if(bonus.equalsIgnoreCase("") || bonus.equalsIgnoreCase("0"))
       		document.add(new Chunk(" "));
       	else
       		document.add(new Chunk("You will be paid a Onetime bonus of Rs. " + bonus + " on your completion of 1 year with Techzert."));
       		//text = "You will be paid a Onetime bonus of Rs. " + bonus + " on your completion of 1 year with Techzert.";//text.replaceAll("BONUS",(String) Object.getPropertyValue("BonusAmount"));
       		

		document.add(Chunk.NEWLINE);
		// -------------------------------------------
		document.add(Chunk.NEWLINE);
		document.add(new Chunk("Effective Date of joining:", timesRomanfont));
		document.add(Chunk.NEWLINE);

		// --------------------
		String property5 = prop.getProperty("db.p5");

		Paragraph paragraph6 = new Paragraph(new Chunk(property5));
		paragraph6.add(new Phrase(/*" DateOfJoining"*/sdf.format(((java.util.GregorianCalendar)Object.getPropertyValue("joiningDate")).getTime())));

		document.add(Chunk.NEWLINE);
	
		document.add(paragraph6);
		// ---------------------

		String property7 = prop.getProperty("db.p6");
		Paragraph paragraph7 = new Paragraph(new Chunk(property7));
		paragraph7.add(/*" ReportingDate. "*/ sdf.format(((java.util.GregorianCalendar)Object.getPropertyValue("reportDate")).getTime()));
		paragraph7.add(". Your appointment will come into effect from your date of joining.");

		document.add(Chunk.NEWLINE);
		document.add(paragraph7);
		document.add(Chunk.NEWLINE);

		// -----------------------

		document.add(new Chunk("Probation", timesRomanfont));

		String property8 = prop.getProperty("db.p7");

		document.add(Chunk.NEWLINE);

		Paragraph paragraph8 = new Paragraph(new Chunk(property8));
		paragraph8.setAlignment(Element.ALIGN_JUSTIFIED);

		document.add(Chunk.NEWLINE);
		document.add(paragraph8);

		// ------------------------------
		document.newPage();
		String property9 = prop.getProperty("db.p8");

		document.add(new Paragraph(new Chunk(property9)));

		// -------------------------
		document.add(Chunk.NEWLINE);
		document.add(new Chunk("Appraisal:", timesRomanfont));
		document.add(Chunk.NEWLINE);

		String property99 = prop.getProperty("db.p9");
		document.add(Chunk.NEWLINE);

		document.add(new Paragraph(new Chunk(property99)));
		document.add(Chunk.NEWLINE);

		// ----------------------

		document.add(new Chunk("Benefits:", timesRomanfont));
		document.add(Chunk.NEWLINE);

		// ----------------

		String property10 = prop.getProperty("db.p10");

		document.add(Chunk.NEWLINE);

		document.add(new Paragraph(new Chunk(property10)));
		document.add(new Chunk("Your Band is "+(String)Object.getPropertyValue("employeeBand")));
		document.add(Chunk.NEWLINE);

		// ------------------

		document.add(Chunk.NEWLINE);
		document.add(new Chunk("Notice Period:", timesRomanfont));
		document.add(Chunk.NEWLINE);

		// -----------------------
		String property11 = prop.getProperty("db.p11");
		String property12 = prop.getProperty("db.p12");

		document.add(new Paragraph(new Chunk(property11)));
		document.add(new Paragraph(new Chunk(property12)));
		document.add(Chunk.NEWLINE);

		// -------------
		document.add(new Chunk("Separation:", timesRomanfont));
		document.add(Chunk.NEWLINE);

		String property13 = prop.getProperty("db.p13");
		String property14 = prop.getProperty("db.p14");

		document.add(Chunk.NEWLINE);

		document.add(new Paragraph(new Chunk(property13)));
		document.add(Chunk.NEWLINE);

		
		Paragraph paragraph = new Paragraph(new Chunk(property14));
		
		paragraph.setAlignment(Element.ALIGN_JUSTIFIED);
		document.add(paragraph);
		document.add(Chunk.NEWLINE);

		// -----------------
		document.add(new Chunk("Retirement:", timesRomanfont));
		document.add(Chunk.NEWLINE);

		String property15 = prop.getProperty("db.p15");
		document.add(Chunk.NEWLINE);

		document.add(new Paragraph(new Chunk(property15)));
		document.add(Chunk.NEWLINE);

		// --------------------
		document.newPage();
		document.add(Chunk.NEWLINE);

		document.add(new Chunk("Service Conditions:", timesRomanfont));
		document.add(Chunk.NEWLINE);

		String property16 = prop.getProperty("db.p16");
		String property17 = prop.getProperty("db.p17");

		document.add(new Paragraph(new Chunk(property16)));
		document.add(new Paragraph(new Chunk(property17)));

		document.add(Chunk.NEWLINE);
		// ------------------------

		document.add(new Chunk("Confidentiality:", timesRomanfont));
		document.add(Chunk.NEWLINE);

		String property18 = prop.getProperty("db.p18");
		document.add(Chunk.NEWLINE);

		document.add(new Paragraph(new Chunk(property18)));
		document.add(Chunk.NEWLINE);
		// ------------------

		document.add(new Chunk("Personal Informatin:", timesRomanfont));
		document.add(Chunk.NEWLINE);

		String property19 = prop.getProperty("db.p19");

		String property20 = prop.getProperty("db.p20");
		String property21 = prop.getProperty("db.p21");

		document.add(Chunk.NEWLINE);
		document.add(new Paragraph(new Chunk(property19)));

		document.add(Chunk.NEWLINE);

		document.add(new Paragraph(new Chunk(property20)));
		document.add(Chunk.NEWLINE);
		document.add(new Paragraph(new Chunk(property21)));
		document.add(Chunk.NEWLINE);

		document.add(new Chunk("Yours Sincerely,"));
		document.add(Chunk.NEWLINE);
		document.add(new Chunk("For Techzert Software Pvt. Ltd."));

		for (int i = 0; i < 5; i++) {
			document.add(Chunk.NEWLINE);

		}

		document.add(new Chunk("_____________________________"));
		document.add(Chunk.NEWLINE);

		document.add(new Chunk("Mukesh Kumar Maharana"));
		document.add(Chunk.NEWLINE);

		document.add(new Chunk("Vice President - Human Resources"));
		document.add(Chunk.NEWLINE);

		document.add(Chunk.NEWLINE);

		document.add(new Chunk("Enclosure:", timesRomanfont));
		document.add(Chunk.NEWLINE);

		List orderedList = new List(List.ORDERED);

		orderedList.add(new ListItem("Annexure I"));

		orderedList.add(new ListItem("Annexure II"));

		document.add(orderedList);

		document.add(Chunk.NEWLINE);

		document.add(new Chunk("I agree to accept the terms and conditions mentioned above."));

		document.add(Chunk.NEWLINE);

		Chunk chunk = new Chunk("Signature: __________________________");

		Paragraph paragraph9 = new Paragraph();
		paragraph9.add(chunk);
		paragraph9.setAlignment(Element.ALIGN_RIGHT);
		document.add(Chunk.NEWLINE);
		document.add(paragraph9);

		document.add(new Chunk("Name:"));

		document.add(Chunk.NEWLINE);

		document.add(new Chunk("Place:"));

		document.add(Chunk.NEWLINE);

		document.add(new Chunk("Date:"));
		// -----------------------

		document.newPage();
		Paragraph paragraph12 = new Paragraph(new Chunk("Annexure I",timesRomanfont));
		paragraph12.setAlignment(Element.ALIGN_CENTER);
		document.add(paragraph12);
		Paragraph paragraph13 = new Paragraph(new Chunk("Compensation & Benefit Structure",timesRomanfont));

		paragraph13.setAlignment(Element.ALIGN_CENTER);
		document.add(paragraph13);
		document.add(Chunk.NEWLINE);
		
		float[] columnWidths = { 2f, 1f, 1f };

		PdfPTable table = new PdfPTable(3);

		PdfPCell cell;
		table.getDefaultCell().setHorizontalAlignment(Element.ALIGN_LEFT);
		table.setWidths(columnWidths);

		cell = new PdfPCell(new Phrase("Name : "+(String) Object.getPropertyValue("employeeName")));
		cell.setColspan(3);
		table.addCell(cell);
		table.addCell(new PdfPCell(new Phrase(" Designation : "+(String) Object.getPropertyValue("designation"), fontcabecatable)));
		table.addCell(new PdfPCell(new Phrase(" Band : "+(String)Object.getPropertyValue("employeeBand"), fontcabecatable)));
		table.addCell(new PdfPCell(new Phrase(" Grade : "+(String)Object.getPropertyValue("employeeGrade"), fontcabecatable)));

		cell = new PdfPCell(new Phrase("-"));
		cell.setColspan(3);
		table.addCell(cell);

		table.addCell(new PdfPCell(new Phrase(" Fixed Componentes (Rs)", fontcabecatable)));
		table.addCell(new PdfPCell(new Phrase("Monthly (Rs)", fontcabecatable)));
		table.addCell(new PdfPCell(new Phrase(" Annual (Rs)", fontcabecatable)));

		table.addCell("Basic");
		String BasicMonthly = new Double((double) Object1.getPropertyValue("BasicMonthly")).toString();
		table.addCell(BasicMonthly);
		String BasicAnnual = new Double((double) Object1.getPropertyValue("BasicAnnual")).toString();
		table.addCell(BasicAnnual);

		table.addCell("House Rent Allowance");
		table.addCell(Object1.getPropertyValue("HRAMonthly").toString());
		table.addCell(Object1.getPropertyValue("HRAAnnual").toString());

		table.addCell("	Special Allowance");
		table.addCell(Object1.getPropertyValue("SpAllowanceMonthly").toString());
		table.addCell(Object1.getPropertyValue("SpAllowanceAnuual").toString());

		table.addCell("	Children Educational Allowance");
		table.addCell(Object1.getPropertyValue("ChildEduAllowanceMonthly").toString());
		table.addCell(Object1.getPropertyValue("ChildEduAllowanceAnnual").toString());

		table.addCell("	Gross Salary -A");
		table.addCell(Object1.getPropertyValue("GrossSalMonthly").toString());
		table.addCell(Object1.getPropertyValue("GrossSalAnnual").toString());

		cell = new PdfPCell(new Phrase(" -"));
		cell.setColspan(3);
		table.addCell(cell);

		cell = new PdfPCell(new PdfPCell(new Phrase(" Retiral Benefits", fontcabecatable)));
		cell.setColspan(3);
		table.addCell(cell);

		table.addCell("	Gratuity (4.8 % of Basic )");
		table.addCell(Object1.getPropertyValue("GratuityMonthly").toString());
		table.addCell(Object1.getPropertyValue("GratuityAnnual").toString());

		table.addCell("	PF ( Employer Contribution ) )");
		table.addCell(Object1.getPropertyValue("EmployerPFMonthly").toString());
		table.addCell(Object1.getPropertyValue("EmployerPFAnnual").toString());

		table.addCell("	Medical Insurance");
		table.addCell(Object1.getPropertyValue("MedInsuranceMonthly").toString());
		table.addCell(Object1.getPropertyValue("MedInsuranceAnnual").toString());

		table.addCell("	ESI Employer (4.75% of Gross Salary)");
		table.addCell(Object1.getPropertyValue("EmployerESIMonthly").toString());
		table.addCell(Object1.getPropertyValue("EmployerESIAnnual").toString());

		table.addCell(new PdfPCell(new Phrase(" Total Retiral Benefits -B", fontcabecatable)));
		table.addCell(Object1.getPropertyValue("TotRetBenefitsMonthly").toString());
		table.addCell(Object1.getPropertyValue("TotRetBenefitsAnnual").toString());

		cell = new PdfPCell(new Phrase(" -"));
		cell.setColspan(3);
		table.addCell(cell);

		cell = new PdfPCell(new PdfPCell(new Phrase(" Statutory Deductions", fontcabecatable)));
		cell.setColspan(3);
		table.addCell(cell);

		table.addCell("	PF ( Employee Contribution 12% of Basic))");
		table.addCell(Object1.getPropertyValue("EmployeePFMonthly").toString());
		table.addCell(Object1.getPropertyValue("EmployeePFAnnual").toString());

		table.addCell("	ESI ( Employee Contribution 1.75% of Gross Salary )");
		table.addCell(Object1.getPropertyValue("EmployeeESIMonthly").toString());
		table.addCell(Object1.getPropertyValue("EmployeeESIAnnual").toString());

		table.addCell("	Professional Tax");
		table.addCell(Object1.getPropertyValue("ProfTaxMonthly").toString());
		table.addCell(Object1.getPropertyValue("ProfTaxAnnual").toString());

		table.addCell(new PdfPCell(new Phrase(" Total Statutory Deductions - C", fontcabecatable)));
		table.addCell(Object1.getPropertyValue("TotDeductionsMonthly").toString());
		table.addCell(Object1.getPropertyValue("TotDeductionsAnnual").toString());

		cell = new PdfPCell(new PdfPCell(new Phrase("  -", fontcabecatable)));
		cell.setColspan(3);
		table.addCell(cell);

		table.addCell(new PdfPCell(new Phrase(" Net Salary ( A- C )", fontcabecatable)));
		table.addCell(Object1.getPropertyValue("NetSalaryMonthly").toString());
		table.addCell(Object1.getPropertyValue("NetSalaryAnnual").toString());

		cell = new PdfPCell(new PdfPCell(new Phrase("  -", fontcabecatable)));
		cell.setColspan(3);
		table.addCell(cell);

		table.addCell(new PdfPCell(new Phrase(" Total Cost The company ( A+B )", fontcabecatable)));
		table.addCell(Object1.getPropertyValue("CTCMonthly").toString());
		table.addCell(Object1.getPropertyValue("CTCAnnual").toString());

		cell = new PdfPCell(new PdfPCell(
				new Phrase(" All Allowances are Subject to Tax as per Income Tax rules\r\n", fontcabecatable)));
		cell.setColspan(3);
		table.addCell(cell);

		document.add(table);

		// ------------------------------

//		Chunk glue = new Chunk(new VerticalPositionMark());

		Phrase phrase7 = new Phrase();

		phrase7.add("Yours Sincerely,");
		phrase7.add(new Chunk(glue));
		phrase7.add("I accept the above Annual compensation ");

		Paragraph paragraph99 = new Paragraph();
		paragraph99.add(phrase7);

		document.add(new Phrase("\n"));
		document.add(paragraph99);

		// ----------------------------

		Phrase phraseA1 = new Phrase();

		phraseA1.add("For Techzert Software Pvt. Ltd.");
		phraseA1.add(new Chunk(glue));
		phraseA1.add("& abide by the company policy ");

		Paragraph paragraphA1 = new Paragraph();
		paragraphA1.add(phraseA1);

		document.add(paragraphA1);
		// --------------------------------------

		Phrase phraseA2 = new Phrase();

		phraseA2.add("_________________________");
		phraseA2.add(new Chunk(glue));
		phraseA2.add("_________________________");

		Paragraph paragraphA2 = new Paragraph();
		paragraphA2.add(phraseA2);
		
		
		document.add(new Phrase("\n"));
		document.add(new Phrase("\n"));

		document.add(paragraphA2);

		// --------------------------------------
		Phrase phraseA3 = new Phrase();

		phraseA3.add("Mukesh Kumar Maharana");

		phraseA3.add(new Chunk(glue));
		phraseA3.add("NAME OF THE CANDIDATE");

		Paragraph paragraphA3 = new Paragraph();
		paragraphA3.add(phraseA3);

		document.add(paragraphA3);

		document.add(new Chunk("Vice President - Human Resources"));
		
		
		//-------------------------TABLE END HERE
		// -------------------

		document.newPage();
		

		Paragraph paragraph10 = new Paragraph(new Chunk("Annexure II", fontcabecatable));

		paragraph10.setAlignment(Element.ALIGN_CENTER);
		document.add(paragraph10);
		Paragraph paragraph11 = new Paragraph(
				new Chunk("Documents to be provided at the time of Joining", fontcabecatable));
		paragraph11.setAlignment(Element.ALIGN_CENTER);

		document.add(paragraph11);
		document.add(Chunk.NEWLINE);
		String property23= prop.getProperty("db.p23");

		String property24 = prop.getProperty("db.p24");
		document.add(new Paragraph(new Chunk(property24)));
		
		document.add(Chunk.NEWLINE);
		

		String property22 = prop.getProperty("db.p22");
		document.add(new Paragraph(new Chunk(property22)));
		document.add(Chunk.NEWLINE);


		document.add(new Paragraph(new Chunk(property23)));
		document.add(Chunk.NEWLINE);

		document.close();
		return pdfFileName;

	}
	

}
