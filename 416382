<?php
defined('BASEPATH') OR exit('No direct script access allowed');

class Home extends CI_Controller {
	
	public function trx()
	{
		$db = $this->load->database('s2', true);
		
		$db->select('*, cash_earnings.year as year');
		$db->join('transactions', 'transactions.receipt_number = cash_earnings.receipt_number');
		$db->where('transactions.receipt_number >=', 32457);
		$db->where('payment_mode', 'cheque');
		//$db->where('transactions.receipt_number <=', 25798);
		//$db->where('transactions.year', 2021);
		//$db->where('transactions.month <=', 6);
		$otherReceipts = $db->get('cash_earnings')->result();
		
		foreach($otherReceipts as $pre) {
			$modeNos[] = $pre->mode_no;
		}

		
		$db->where('fee_payments.receipt_number >=', 32457);
		$db->where('fee_payments.mode_no !=', '');
		$db->join('student_details', 'student_details.adm_no = fee_payments.received_from');
		$pres = $db->where_in('mode_no', $modeNos)->get('fee_payments')->result();
		
		?>
		<table>
			<?php foreach($pres as $rct) {?>
			<tr>
				<td>
					<?php echo $rct->day .'/'. $rct->month .'/'. $rct->year?>
				</td>
				<td>
					<?php echo $rct->adm_no ?>
				</td>
				<td>
					<?php echo $rct->name ?>
				</td>
				<td>
					<?php echo $rct->receipt_number ?>
				</td>
				<td>
					<?php echo $rct->mode_no ?>
				</td>
				<td>
					<?php echo $rct->amount ?>
				</td>
			</tr>
			<?php } ?>
		</table>
		<?php
	}
	
	public function income()
	{
		$db = $this->load->database('s2', true);
		
		$students = $db->where('student_status', 1)->get('student_details')->result();
		
		$arrears = $db->where('year', 2020)->get('student_arrears')->result();
		foreach($arrears as $arr) {
			$arrs[$arr->student_id] = $arr->billed_amount;
		}
		
		$prepays = $db->where('year', 2020)->get('student_prepayments')->result();
		foreach($prepays as $pre) {
			$preps[$pre->student_id] = $pre->remaining_amount;
		}
		
		$payments = $db->join('sp_student_details', 'student_details.adm_no = sp_fee_payments.received_from')->where('year', 2021)->where('month <=',6)->order_by('month')->order_by('month')->get('sp_fee_payments')->result();
		
		$pays = [];
		foreach($payments as $pay) {
			if(!isset($pays[$pay->student_id])) {
				$pays[$pay->student_id] = 0;
			}
			
			$pays[$pay->student_id] += $pay->amount;
		}
		
		?>
		<table>
		<?php
		foreach($students as $student) {
			$pr = $preps[$student->student_id] ?? 0;
			$ar = $arrs[$student->student_id] ?? 0;
			$pa = $pays[$student->student_id] ?? 0;
			
			$balance = ($pa + $pr) - ($ar + 38000);
			
			echo "<tr>
				<td>".$student->adm_no."</td>
				<td>".$student->name."</td>
				<td>".$balance."</td>
			</tr>";
		}
		?>
		</table>
		<?php
	}
	
	public function ledger()
	{
		/*
		$accs = [
			['Admin Costs', 1249747, 1052398, 3819114,3332525],
			['Advance', 0, 0,0,0],
			['Activities', 0, 0,0,90373],
			['Arrears', 0, 1006273,0,3527674],
			['BES', 9563223, 15725566,45769408,43678283],
			['Bursaries', 1757415, 1757415,4525747,4525747],
			['Caution', 0, 0,0,0],
			['Contigencies', 0, 0,0,0],
			['Creditors', 100000, 0,240066,0,3],
			['EWC', 1134685, 4117487,5017542,7314943],
			['Farm', 138400, 5480,1278565,5480],
			['Imprest', 57000, 0,172000,0],
			['Insurance', 0, 0,265551,0],
			['Vehicle Insurance', 0, 755127,0,1266674],
			['LTT', 694988, 1101156,4207456,2446614],
			['Mattress', 0, 0,0,0],
			['Medical', 207130, 400001,596720,664989],
			['NHIF', 65900, 65900,197700,0],
			['NSSF', 101830, 101830,610980,0],
			['PAYE', 44680, 44680,237045,0],
			['PE', 1676138, 3014127,8140086,8091523],
			['Prepayments', 0, 1046234,0,3776365],
			['Recoveries', 0, 0,0,0],
			['RMI', 436832, 1623063,4323462,2925671],
			['Uniform', 0, 0,0,0],
			['PTA Fee', 0, 5390541,0,6556728],
			['Damages', 0, 34000,0,135214],
			['Bank Charges', 0, 0,0,0],
			['Infrastructure', 7063339.66, 0,7063339.66,0],
			['Union', 0, 0,40000,0],
			['Welfare', 457241, 0,988026,0],
			['Refund', 0, 4000,0,4000],
			['SMASSE', 0, 0,9000,0],
		];
		*/
		
		/*
		$accs = [
			['Covid 19', 150000, 0, 976440,1200000.85,11,1],
			['RMI', 70590, 643360,3192800,6352200.50],
			['Activities', 0, 884620,49900,6898460.50],
			['LT&T', 0, 643360,1178359,6657200.50],
			['EWC', 700000, 884620,4321045.90,884620],
			['P.E', 0, 2981840,2174221,8995680.50],
			['Welfare', 0, 0,300000,0],
			['Infrastructure', 7630020, 13422300,0,0],
			['Admin Costs', 1400000, 804200,4235395,804200],
			['Creditors', 0, 0,600000,0],
			['Insurance', 0, 0,73428,0],
		];
		*/
		
		/*
		$accs = [
			['Teaching Material', 0, 816615, 2300000,5975088.25],
			['Lab Equipment', 900000, 0, 3690000,0],
			['Internal Exams', 0, 0, 0,0],
			['Library Reference', 0, 0, 0,0],
			['Chalk', 0, 0, 0,0],
			['Text Books', 0, 0, 0,0],
		];
		*/
		
		$months = ['July 31','Aug 31','Sep 30','Oct 31','Nov 30'];
		
		foreach($accs as $key => $acc) {
			echo $acc[0];
			$j = $key + 1;
			echo '('.$j.')';
			?>
			<table style="border-collapse:collapse">
				<tr>
					<td style="border:1px solid grey">2021</td>
					<td style="border:1px solid grey">C/B</td>
					<td style="border:1px solid grey"></td>
					<td style="border:1px solid grey" colspan="2">Debit</td>
					<td style="border:1px solid grey">2021</td>
					<td style="border:1px solid grey">C/B</td>
					<td style="border:1px solid grey"></td>
					<td style="border:1px solid grey" colspan="2">Credit</td>
				</tr>
				<?php 
				$counter = 1;
				
				$curD = $acc[1];
				$curD1 = $acc[1];
				
				$curC = $acc[2];
				$curC1 = $acc[2];
				
				$maxD = $acc[3];
				$maxC = $acc[4];
				
				$diffD = $curD;
				$diffC = $curC;
				
				$maxDD = 11;
				$maxCD = 11;
				foreach($months as $month) {
					
					
					$diff1 = ($maxD - $curD) / (11 - $counter);
					$diff2 = ($maxC - $curC) / (11 - $counter);
					if($counter > 1) {
						$diffD = rand(round($diff1/2), round($diff1 *2));
						
						$curD += $diffD;
						if($curD > $maxD) {
							$diffD = $maxD - $curD1;
							$curD = $maxD;
						}
						
						$curD1 = $curD;
						
						$diffC = rand(round($diff2/2), round($diff2 *2));
						
						$curC += $diffC;
						if($curC > $maxC) {
							$diffC = $maxC - $curC1;
							$curC = $maxC;
						}
						
						$curC1 = $curC;
					}
				?>
				<tr>
					<td style="border:1px solid grey;width:60px"><?php echo $month ?></td>
					<td style="border:1px solid grey;width:20px">"</td>
					<td style="border:1px solid grey;width:20px"><?php echo $counter?></td>
					<td style="border:1px solid grey;width:80px"><?php echo number_format($diffD)?></td>
					<td style="border:1px solid grey;width:80px"><b><?php echo number_format($curD)?></b></td>
					<td style="border:1px solid grey;width:60px"><?php echo $month ?></td>
					<td style="border:1px solid grey;width:20px">"</td>
					<td style="border:1px solid grey;width:20px"><?php echo $counter?></td>
					<td style="border:1px solid grey;width:80px"><?php echo number_format($diffC)?></td>
					<td style="border:1px solid grey;width:80px"><b><?php echo number_format($curC)?></b></td>
				</tr>
			<?php 
			$counter++;
			} ?>
			</table>
			<br><br>
			<p style="page-break-after:always"></p>
			<?php
		}
	}
	
	public function attendance()
	{
		$db = $this->load->database('odbc', true);

		$details = $db->query('SELECT att.*, att.CHECKINOUTCHECKTIME FROM att ORDER BY att.CHECKINOUTCHECKTIME')->result();
		
		foreach($details as $detail) {
			$grouped[round($detail->USERID)]['name'] = $detail->Name;
			
			$day = date("d",strtotime($detail->CHECKINOUTCHECKTIME));
			if($detail->CHECKTYPE == 'I') {
				$grouped[round($detail->USERID)]['in'][$day] = $detail->CHECKINOUTCHECKTIME;
			} else {
				$grouped[round($detail->USERID)]['out'][$day] = $detail->CHECKINOUTCHECKTIME;
			}
		}
		
		?>
		<table style="page-break-after:always;width:100%;border-collapse:collapse">
			<tr>
				<td style="width:120px"><img src="<?php echo base_url('images/jobo.png')?>"></td>
				<td align="center">
					<b>ST JOSEPH'S BOYS HIGH SCHOOL</b><br>
					<b>PO BOX 485-30200 KITALE (KENYA)</b><br>
					Tel: 0708992590 Email: infostjosephs@gmail.com
				</td>
			</tr>
			<tr>
			<td colspan="2"><b>STAFF ATTENDANCE SUMMARY</b>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;GROUP: <b>TEACHING STAFF</b>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;DATE <b>01/01/2021</b> - <b>31/01/2021</b> </td>
			</tr>
			<tr>
				<td style="border:1px solid grey"><center>#</center></td>
				<td style="border:1px solid grey">NAME</td>
				<td style="border:1px solid grey">CHECK INS</td>
				<td style="border:1px solid grey">CHECK OUTS</td>
			</tr>
			<?php
			$c = 0;
			foreach($grouped as $userId => $dets) {
				$c++;
				?>
				<tr style="page-break-inside: avoid">
					<td style="border:1px solid grey"><?php echo $c?></td>
					<td style="border:1px solid grey"><?php echo $dets['name']?></td>
					<td style="border:1px solid grey"><?php echo isset($dets['in']) ? count($dets['in']) : '--'?></td>
					<td style="border:1px solid grey"><?php echo isset($dets['out']) ? count($dets['out']) : '--'?></td>
				</tr>
				<?php
			}
			?>
		</table>
		<?php
		
		//dump($grouped);
		exit;
		
		//Grouped by day
		
		foreach($details as $detail) {
			$day = date("d",strtotime($detail->CHECKINOUTCHECKTIME));
			$grouped[$day][round($detail->USERID)]['name'] = $detail->Name;
			
			if($detail->CHECKTYPE == 'I') {
				$grouped[$day][round($detail->USERID)]['in'] = $detail->CHECKINOUTCHECKTIME;
			} else {
				$grouped[$day][round($detail->USERID)]['out'] = $detail->CHECKINOUTCHECKTIME;
			}
		}
		
		
		
		foreach($grouped as $day => $staffs) {
			?>
			<table style="page-break-after:always;width:100%;border-collapse:collapse">
				<tr>
					<td style="width:120px"><img src="<?php echo base_url('images/jobo.png')?>"></td>
					<td align="center">
						<b>ST JOSEPH'S BOYS HIGH SCHOOL</b><br>
						<b>PO BOX 485-30200 KITALE (KENYA)</b><br>
						Tel: 0708992590 Email: infostjosephs@gmail.com
					</td>
				</tr>
				<tr>
				<td colspan="2"><b>STAFF DAILY ATTENDANCE</b>   GROUP: <b>TEACHING STAFF</b>&nbsp;&nbsp;&nbsp;DATE <b></b> <?php echo $day?>/01/2021</td>
				</tr>
				<tr>
					<td style="border:1px solid grey"><center>#</center></td>
					<td style="border:1px solid grey">NAME</td>
					<td style="border:1px solid grey">TIME IN</td>
					<td style="border:1px solid grey">TIME OUT</td>
				</tr>
				<?php
				$c = 0;
				foreach($staffs as $staf) {
					$c++;
					?>
					<tr>
						<td style="border:1px solid grey"><?php echo $c?></td>
						<td style="border:1px solid grey"><?php echo $staf['name']?></td>
						<td style="border:1px solid grey"><?php echo isset($staf['in']) ? $staf['in'] : ''?></td>
						<td style="border:1px solid grey"><?php echo isset($staf['out']) ? $staf['out'] : ''?></td>
					</tr>
					<?php
				}
				?>
			</table>
			<?php
		}
	}
	
	public function transactions()
	{
		$this->db->order_by('type');
		$this->db->where('account_id', 1);
		$accounts = $this->db->get('ledger_accounts')->result();
		
		foreach($accounts as $acc) {
			$accs[$acc->id] = $acc;
		}
		
		$this->db->select('*');
		$this->db->where('account_id', 1);
		$this->db->where('status', 1);
		$this->db->where('id >=', 21882);
		//$this->db->where('month >=', 1);
		//$this->db->where('year >=', 2023);
		//$this->db->where('type', 'Withdrawal');
		///$this->db->join('payments','payments.transaction_id = transactions.id');
		///$this->db->where('transactions.transaction_date <=', '2020-12-31');
		///$this->db->where('transactions.transaction_date >=', '2020-01-01');
		$transactions = $this->db->get('transactions')->result();
		
		foreach($transactions as $tr) {
			$ids[] = $tr->id;
		}
		
		foreach($this->db->where_in('transaction_id', $ids)->get('ledger_transactions')->result() as $ltr) {
			$ledgerTrans[$ltr->transaction_id][$ltr->acc_id] = $ltr->debit + $ltr->credit;
		}
		
		
		
		?>
		<style>
		td {
			border:1px solid grey
		}
		</style>
		<table style="border-collapse:collapse">
			<tr>
				<td>Date</td>
				<td>Particulars</td>
				<td>VR</td>
				<td>Cheque No</td>
				<?php
				foreach($accs as $acc) {
					?>
					<td><?php echo $acc->abbreviation?></td>
					<?php
				}
				?>
			</tr>
		<?php
		foreach($transactions as $transaction) {
			?>
			<tr>
				<td><?php echo $transaction->day.'-'.$transaction->month.'-'.$transaction->year?></td>
				<td><?php echo str_replace('Payment to ','', $transaction->transaction_description) ?></td>
				<td><?php echo $transaction->doc_number ?></td>
				<td><?php echo $transaction->mode_number ?></td>
				<?php
				foreach($accs as $key => $acc) {
					?>
					<td><?php echo $ledgerTrans[$transaction->id][$acc->id]?></td>
					<?php
				}
				?>
			</tr>
			<?php
		}
		?>
		</table>
		<?php
	}
	
	public function arrears()
	{
		$db = $this->load->database('s2', true);
		
		$db->group_by('vote_heads.vote_id');
		$db->where('year', 2019);
		$db->join('vote_heads', 'vote_heads.vote_id = student_invoices.vote_id');
		$votePays = $db->get('student_invoices')->result();
		
		foreach($votePays as $v) {
			$voteNames[$v->vote_id] = $v;
		}
		
		$streams = $db->get('streams')->result();
		
		foreach($streams as $stream) {
			$streamIds[$stream->stream_id] = $stream->stream_name;
		}
		
		$students = $db->get('student_details')->result();
		
		foreach($students as $student) {
			$studentIds[$student->student_id] = $student;
		}
		
		$arrears = $db->where('year', 2019)->get('student_arrears')->result();
		
		foreach($arrears as $arrear) {
			$studentArrears[$arrear->student_id] = $arrear->billed_amount - $arrear->paid_amount;
		}
		
		$arrears = $db->where('year', 2019);
		$prepayments = $db->get('student_prepayments')->result();
		foreach($prepayments as $prepay) {
			$studentPrepayments[$prepay->student_id] = $prepay->remaining_amount;
		}
		
		$myPays = [];
		foreach($votePays as $votePay) {
			$db->where('year', 2019);
			$db->where('vote_id', $votePay->vote_id);
			$db->select('student_id, year');
			$db->select_sum('billed_amount');
			$db->select_sum('paid_amount');
			$db->select_sum('unpaid_amount');
			$db->group_by('student_id');
			$vPays = $db->get('student_invoices')->result();
			
			foreach($vPays as $vPay) {
				$myPays[$studentIds[$vPay->student_id]->class][$streamIds[$studentIds[$vPay->student_id]->stream]][$vPay->student_id]['student_id'] = $studentIds[$vPay->student_id]->student_id;
				$myPays[$studentIds[$vPay->student_id]->class][$streamIds[$studentIds[$vPay->student_id]->stream]][$vPay->student_id]['adm_no'] = $studentIds[$vPay->student_id]->adm_no;
				$myPays[$studentIds[$vPay->student_id]->class][$streamIds[$studentIds[$vPay->student_id]->stream]][$vPay->student_id]['name'] = $studentIds[$vPay->student_id]->name;
				$myPays[$studentIds[$vPay->student_id]->class][$streamIds[$studentIds[$vPay->student_id]->stream]][$vPay->student_id]['votes'][$votePay->vote_id] = $vPay;
			}
		}
		
		foreach($myPays as $class => $classPays) {
			if($class == 5) {
			foreach($classPays as $stream => $streamPays) {
				echo '<span style="font-size:14px"><b>FORM '.($class - 1).' '.$stream.' FEE REGISTERS 2019<b></span>';
				?>
				<table style="border-collapse:collapse;table-layout:fixed;font-size:11px">
					<tr>
						<td style="border:1px solid grey"><b></b></td>
						<td style="border:1px solid grey"><b></b></td>
						<td style="border:1px solid grey"><b></b></td>
						<td style="border:1px solid grey"><b></b></td>
						<?php
							foreach($voteNames as $mVote) {
								?>
								<td colspan="2" style="border:1px solid grey"><center><b><?php echo $mVote->vote_abb?></b></center></td>
								<?php
							}
						?>
						<td style="border:1px solid grey"><b></b></td>
						<td style="border:1px solid grey"><b></b></td>
					</tr>
					<tr>
						<td style="border:1px solid grey"><b>#</b></td>
						<td style="border:1px solid grey"><b>Adm#</b></td>
						<td style="border:1px solid grey"><b>Name</b></td>
						<td style="border:1px solid grey"><b>Arrears</b></td>
						<?php
							foreach($voteNames as $mVote) {
								?>
								<td style="border:1px solid grey"><center><b>Paid</b></center></td>
								<td style="border:1px solid grey"><center><b>Bal</b></center></td>
								<?php
							}
						?>
						<td style="border:1px solid grey"><b><center>Prepaid</center></b></td>
						<td style="border:1px solid grey"><b><center>Balance</center></b></td>
					</tr>
				<?php
					$counter = 1;
					$totalArrears = 0;
					$totalPrepayments = 0;
					$totalBalances = 0;
					$votesBal = [];
					foreach($streamPays as $pay) {
						$balance = 0;
						$balance += $studentArrears[$pay['student_id']];
						$balance -= $studentPrepayments[$pay['student_id']];
						
						?>
							<tr>
							<td style="border:1px solid grey"><?php echo $counter?></td>
							<td style="border:1px solid grey"><?php echo $pay['adm_no']?></td>
							<td style="border:1px solid grey;width:250px"><?php echo $pay['name']?></td>
							<td style="border:1px solid grey;width:80px;text-align:right"><?php echo $studentArrears[$pay['student_id']] ?></td>
							<?php
								foreach($voteNames as $mVote) {
									$balance += $pay['votes'][$mVote->vote_id]->unpaid_amount;
									
									$votesBal['paid'][$mVote->vote_id] += $pay['votes'][$mVote->vote_id]->paid_amount;
									$votesBal['bal'][$mVote->vote_id] += $pay['votes'][$mVote->vote_id]->unpaid_amount;
									?>
									<td style="border:1px solid grey;width:70px;text-align:right"><?php echo number_format($pay['votes'][$mVote->vote_id]->paid_amount)?></td>
									<td style="border:1px solid grey;width:70px;text-align:right"><?php echo number_format($pay['votes'][$mVote->vote_id]->unpaid_amount)?></td>
									<?php
								}
							?>
							<td style="border:1px solid grey;width:80px;text-align:right"><?php echo $studentPrepayments[$pay['student_id']]?></td>
							<td style="border:1px solid grey;width:80px;text-align:right"><b><?php echo number_format($balance)?></b></td>
							</tr>
						<?php
						
						$totalArrears += $studentArrears[$pay['student_id']];
						$totalPrepayments += $studentPrepayments[$pay['student_id']];
						$totalBalances += $balance;
						$counter++;
						
						if($counter == 58) {
							?>
							</table>
							<p style="page-break-after:always"></p>
							<table style="border-collapse:collapse;table-layout:fixed;font-size:11px">
								<tr>
						<td style="border:1px solid grey"><b></b></td>
						<td style="border:1px solid grey"><b></b></td>
						<td style="border:1px solid grey"><b></b></td>
						<td style="border:1px solid grey"><b></b></td>
						<?php
							foreach($voteNames as $mVote) {
								?>
								<td colspan="2" style="border:1px solid grey"><center><b><?php echo $mVote->vote_abb?></b></center></td>
								<?php
							}
						?>
						<td style="border:1px solid grey"><b></b></td>
						<td style="border:1px solid grey"><b></b></td>
					</tr>
					<tr>
						<td style="border:1px solid grey"><b>#</b></td>
						<td style="border:1px solid grey"><b>Adm#</b></td>
						<td style="border:1px solid grey"><b>Name</b></td>
						<td style="border:1px solid grey"><b>Arrears</b></td>
						<?php
							foreach($voteNames as $mVote) {
								?>
								<td style="border:1px solid grey"><center><b>Paid</b></center></td>
								<td style="border:1px solid grey"><center><b>Bal</b></center></td>
								<?php
							}
						?>
						<td style="border:1px solid grey"><b><center>Prepaid</center></b></td>
						<td style="border:1px solid grey"><b><center>Balance</center></b></td>
					</tr>
							<?php
						}
					}
					?>
					<tr>
						<td style="border:1px solid grey"></td>
						<td style="border:1px solid grey"></td>
						<td style="border:1px solid grey;width:250px"></td>
						<td style="border:1px solid grey;width:80px;text-align:right"><b><?php echo $totalArrears ?></b></td>
						<?php
							foreach($voteNames as $mVote) {
								$balance += $pay['votes'][$mVote->vote_id]->unpaid_amount;
								?>
								<td style="border:1px solid grey;width:70px;text-align:right"><b><?php echo number_format($votesBal['paid'][$mVote->vote_id])?></b></td>
								<td style="border:1px solid grey;width:70px;text-align:right"><b><?php echo number_format($votesBal['bal'][$mVote->vote_id])?></b></td>
								<?php
							}
						?>
						<td style="border:1px solid grey;width:80px;text-align:right"><b><?php echo $totalPrepayments ?></b></td>
						<td style="border:1px solid grey;width:80px;text-align:right"><b><b><?php echo $totalBalances?></b></b></td>
					</tr>
					<?php
				?>
				</table>
				<p style="page-break-after:always"></p>
				<?php
			}
			}
		}
	}
	
	public function index()
	{
		$this->load->helper('global_helper');
		$db = $this->load->database('s2', true);
		
		$db->where('account_id', 6);
		$db->join('vote_heads', 'vote_heads.vote_id = ledger_folio.vote_id');
		$voteHeads = $db->get('ledger_folio')->result();
		
		foreach($voteHeads as $head) {
			$ledgerFolio[$head->L_folio] = $head->vote_abb;
		}
		
		$this->db->select('*, cash_earnings.year as year');
		$db->join('transactions', 'transactions.receipt_number = cash_earnings.receipt_number');
		$db->where('transactions.receipt_number >=', 37989);
		//$db->where('transactions.receipt_number <=', 25798);
		//$db->where('transactions.year', 2021);
		//$db->where('transactions.month <=', 6);
		$otherReceipts = $db->get('cash_earnings')->result();
		
		foreach($otherReceipts as $rct) {
			$receiptsTrans[$rct->receipt_number] = $rct;
		}
		
		$this->db->select('*, fee_payments.year as year');
		$db->join('transactions', 'transactions.receipt_number = fee_payments.receipt_number');
		$db->where('transactions.receipt_number >=', 37989);
		//$db->where('transactions.receipt_number <=', 25798);
		//$db->where('transactions.year', 2021);
		//$db->where('transactions.month <=', 6);
		$feesReceipts = $db->get('fee_payments')->result();
		
		foreach($feesReceipts as $rct) {
			$receiptsFees[$rct->receipt_number] = $rct;
		}
		
		$this->db->select('*, transactions.year as year');
		//$this->db->select('*');
		//$db->where('receipt_number >', 25798);
		//$db->where('receipt_number <=', 25798);
		$db->where('status', 1);
		//$db->where('year', 2021);
		//$db->where('month <=', 6);
		$db->where('transactions.receipt_number >=', 37989);
		//$db->where('transactions.receipt_number <=', 25798);
		$db->where('transactions.account_id', 6);
		$db->where('journal.L_folio !=', 'Cash');
		$db->where('journal.L_folio !=', 'Bank');
		$db->join('transactions','transactions.transaction_ref = journal.transaction_ref');
		$ledgerTransactions = $db->get('journal')->result();
		
		foreach($ledgerTransactions as $ledger) {
			$ledgerTrans[$ledger->transaction_ref][] = $ledger;
		}
		
		//transactions
		//$db->where('receipt_number >', 25798);
		//$db->where('receipt_number <=', 25798);
		$this->db->select('*, transactions.year as year');
		$db->where('status', 1);
		//$db->where('year', 2021);
		//$db->where('month <=', 6);
		$db->where('account_id', 6);
		$db->where('transactions.receipt_number >=', 37989);
		//$db->where('transactions.receipt_number <=', 25798);
		//$db->where('transaction_type', 'Fees');
		//$db->select_sum('amount');
		$transactions = $db->get('transactions')->result();
		//dump($transactions);
		//exit;
		
		$groupedTransactions = [];
		$total = 0;
		$total3 = 0;
		$total4 = 0;
		$totalCredits = 0;
		$totalDebits = 0;
		
		foreach($transactions as $transaction) {
			$total += $transaction->amount;
			$date = $transaction->year.'_'.$transaction->month.'_'.$transaction->day;
			
			if($transaction->transaction_type != 'Fees') {
				if(!isset($groupedTransactions[$date]['OtherReceipts'])) {
					$groupedTransactions[$date]['OtherReceipts'] = [];
				}
				
				$groupie = [
					'description' => $receiptsTrans[$transaction->receipt_number]->received_from,
					'rct_no' => $transaction->receipt_number,
					'cash' => in_array($transaction->payment_mode, ['cash','money_order']) ? $transaction->amount : 0,
					'bank' => !in_array($transaction->payment_mode, ['cash','money_order']) ? $transaction->amount : 0
				];
				
				foreach($ledgerTrans[$transaction->transaction_ref] as $trns) {
					$groupie['folios'][$trns->L_folio]['Debit'] = $trns->Debit;
					$groupie['folios'][$trns->L_folio]['Credit'] = $trns->Credit;
					$totalCredits += $trns->Credit;
					$totalDebits += $trns->Debit;
				}
				
				$groupedTransactions[$date]['OtherReceipts'][] = $groupie;
			} else {
				if(!isset($groupedTransactions[$date]['Fees'])) {
					$groupedTransactions[$date]['Fees']['cash'] = 0;
					$groupedTransactions[$date]['Fees']['bank'] = 0;
					$groupedTransactions[$date]['Fees']['folios'] = [];
					$groupedTransactions[$date]['Fees']['folios'] = [];
					$groupedTransactions[$date]['Fees']['bg_rct_no'] = $transaction->receipt_number;
				}
				
				$fee = [
					'description' => 'STUDENTS',
					'rct_no' => $transaction->receipt_number,
					'bg_rct_no' => $groupedTransactions[$date]['Fees']['bg_rct_no'],
					'end_rect_no' => '',
					'cash' => in_array($transaction->payment_mode, ['cash','money_order']) ? $groupedTransactions[$date]['Fees']['cash'] + $transaction->amount : $groupedTransactions[$date]['Fees']['cash'],
					'bank' => !in_array($transaction->payment_mode, ['cash','money_order']) ? $groupedTransactions[$date]['Fees']['bank'] + $transaction->amount : $groupedTransactions[$date]['Fees']['bank']
				];
				
				foreach($ledgerTrans[$transaction->transaction_ref] as $trns) {
					if(!isset($fee['folios'][$trns->L_folio])) {
						$fee['folios'][$trns->L_folio] = [
							'Debit' => 0,
							'Credit' => 0
						];
					}
					
					/**
					if(!isset($groupedTransactions[$date]['Fees']['folios'][$trns->L_folio])) {
						$groupedTransactions[$date]['Fees']['folios'][$trns->L_folio]['Credit'] = 0;
						$groupedTransactions[$date]['Fees']['folios'][$trns->L_folio]['Debit'] = 0;
					}
					*/
					
					$fee['folios'][$trns->L_folio]['Debit'] += $trns->Debit;
					$fee['folios'][$trns->L_folio]['Credit'] += $trns->Credit;
					
					$totalCredits += $trns->Credit;
					$totalDebits += $trns->Debit;
				}
				
				$groupedTransactions[$date]['Fees'][] = $fee;
			}
		}
		
		foreach($groupedTransactions as $date => $trans1) {
			$folios[$date] = [];
			foreach($trans1 as $key1 => $trans2) {
				if($key1 == 'Fees') {
					foreach($trans2 as $key2 => $trans3) {
						if(is_int($key2) && is_array($trans3)) {
							foreach($trans3['folios'] as $folio => $tr) {
								if(!isset($folios[$date][$folio])) {
									$folios[$date][$folio]['Credit'] = 0;
									$folios[$date][$folio]['Debit'] = 0;
								}
								
								$folios[$date][$folio]['Credit'] += $tr['Credit'];
								$folios[$date][$folio]['Debit'] += $tr['Debit'];
							}
						}
					}
				}
			}
		}
		
		foreach($folios as $date => $folios) {
			$groupedTransactions[$date]['Fees']['folios'] = $folios;
		}
		
		//dump($groupedTransactions);
		//dump($folios);
		//exit;
		$myTransactions = [];
		$total2 = 0;

		foreach($groupedTransactions as $date => $gTrans) {
			$date2 = explode('_', $date);
			$date3 = $date2[2].'/'.$date2[1].'/'.'2022';
			
			foreach($gTrans as $key => $trans) {
				if($key == 'OtherReceipts') {
					foreach($trans as $tran) {
						$myTransactions[] = [
							'date' => $date3,
							'description' => $tran['description'],
							'rct_no' => $tran['rct_no'],
							'cash' => $tran['cash'],
							'bank' => $tran['bank'],
							'folios' => $tran['folios']
						];
						$total2 += $tran['bank'];
					}
				} else {
					$banky = 0;
					$cashy = 0;
					$rct_noo = null;
					foreach($trans as $tran) {
						if(is_array($tran) && isset($tran['description'])) {
							$banky += $tran['bank'];
							$cashy += $tran['cash'];
							$rct_noo = $tran['rct_no'];
						}
					}
					$myTransactions[] = [
						'date' => $date3,
						'description' => $trans['description'],
						'rct_no' => $trans['bg_rct_no'].' - '.$rct_noo,
						'cash' => $cashy,
						'bank' => $banky,
						'folios' => $trans['folios']
					];
				}
			}
		}
		
		$myTotal = 0;
		foreach($myTransactions as $transs) {
			$myTotal += $transs['cash'] + $transs['bank'];
		}
		
		//echo $total.'--'.$myTotal;
		//echo '<br>'.$totalCredits.'--'.$totalDebits;
		///echo $total.'--'.$total2.'--'.$total3.'--'.$total4;
		//exit;
		
		
		?>
		<table style="border-collapse: collapse">
			<tr>
				<td style="border:1px solid black">Date</td>
				<td style="border:1px solid black">Particulars</td>
				<td style="border:1px solid black">Rct No</td>
				<td style="border:1px solid black">Cash</td>
				<td style="border:1px solid black">Bank</td>
				<td style="border:1px solid black">Total</td>
				<?php
				foreach($ledgerFolio as $folio) {
					echo '<td style="border:1px solid black">'.$folio.'</td>';
				}
				?>
			</tr>
				<?php
				foreach($myTransactions as $transaction) {
					?>
					<tr>
						<td style="border:1px solid black"><?php echo $transaction['date'] ?></td>
						<td style="border:1px solid black"><?php echo $transaction['description'] ?></td>
						<td style="border:1px solid black"><?php echo $transaction['rct_no'] ?></td>
						<td style="border:1px solid black"><?php echo $transaction['cash'] ?></td>
						<td style="border:1px solid black"><?php echo $transaction['bank'] ?></td>
						<td style="border:1px solid black"><?php echo $transaction['cash'] + $transaction['bank'] ?></td>
						<?php
						foreach($ledgerFolio as $key => $folio) {
							?><td style="border:1px solid black"><?php echo isset($transaction['folios'][$key]) ? $transaction['folios'][$key]['Debit'] : '0'?></td><?php
						}
						?>
					</tr>
					<?php
				}
			?>
		</table>
		<?php
	}
	
	public function prepaymentsList()
	{
		$this->load->helper('global_helper');
		$db = $this->load->database('s2', true);
		
		$db->where('remaining_amount >', 0);
		$db->join('student_details','student_details.student_id = student_prepayments.student_id');
		$db->join('streams','streams.stream_id = student_details.stream');
		$db->where('year', 2019);
		$prepayments = $db->get('student_prepayments')->result();
		$counter = 1;
		$total = 0;
		?>
		<style>
		td {
			border:1px solid black;
		}
		</style>
		<table style="border-collapse:collapse;width:100%">
			<tr>
				<td><b>#</b></td>
				<td><b>Adm No</b></td>
				<td><b>Name</b></td>
				<td><b>Class</b></td>
				<td><b>Stream</b></td>
				<td><b>Amount</b></td>
			</tr>
		<?php
		foreach($prepayments as $pre) {
			?>
			<tr>
				<td><?php echo $counter?></td>
				<td><?php echo $pre->adm_no?></td>
				<td><?php echo $pre->name?></td>
				<td><?php echo $pre->class?></td>
				<td><?php echo $pre->stream_name?></td>
				<td style="text-align:right"><?php echo number_format($pre->remaining_amount)?></td>
			</tr>
			<?php
			$counter++;
			$total += $pre->remaining_amount;
		}
		?>
			<tr>
				<td><b></b></td>
				<td><b></b></td>
				<td><b></b></td>
				<td><b></b></td>
				<td><b>Total</b></td>
				<td><b><?php echo number_format($total)?></b></td>
			</tr>
		</table>
		<?php
	}
}
