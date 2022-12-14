# Toan-tu-3-ngoi
datagripdt
----
import React, { useState, useContext } from 'react'
import { DataGrid, GridColDef, jaJP, GridColumns } from '@mui/x-data-grid';
import { Button, Grid, Link as MuiLink } from '@mui/material';
import Box from '@mui/material/Box';
import NextLink from "next/link";
import { StyledDataGrid } from "../../styles/Styled";
import { ConfirmDiallogProps, ConfirmDiallog } from '../Common/ComfirmDialog';
import { DialogDispContext } from '../../pages/CallCenter/CallBack/index';

const DataGrid_list = (props) => {

  const [confirmConfig, setConfirmConfig] = useState<ConfirmDiallogProps | undefined>()
  const [pageSize, setPageSize] = React.useState<number>(10);
  const [diaEditFlg, setDiaEditFlg] = useContext(DialogDispContext);
  const [diaDetailFlg, setDiaDetailFlg] = useState(false);
  const handleEditClick = (props) => {
    console.log(props);
    setDiaEditFlg(true);
  };
  const handleDetailClick = (props) => {
    setDiaEditFlg(false);
  };
  const handleShowDetail = (props) => {
    console.log(props);
    setDiaEditFlg(false);
    setDiaDetailFlg(true);
  };

  // 改行コード置換（\r\nを<BR/>に）
  const replaceContent = (content) => {
    const str = content.split(/(\r\n)/).map((item, index) => {
      return (
        <React.Fragment key={index}>
          {item.match(/\r\n/) ? <br /> : item}
        </React.Fragment>
      );
    });
    return <div style={{ textAlign: 'center' }}>{str}</div>
  }

  const columns = React.useMemo<GridColumns>(
    () => [
      // {
      //   field: 'getButton',
      //   headerName: ' ',
      //   flex: 0.3,
      //   renderCell: (params) => {
      //     return  (
      //       <Button
      //         variant="contained"
      //         color="primary"
      //         size="small"
      //         onClick={() => handleEditClick(params.row.seqNo)}
      //       >変更
      //       </Button>
      //     )
      //   }
      // },

      {
        field: 'id', headerName: '', type: 'string', width: 50, headerAlign: 'center', align: 'center', flex: 0.5,
        renderCell: (params) => {
          if (!params.row.datFlg) {
            return <NextLink
              //  href='/'
              href={{ pathname: '/Edit', query: { id: params.row.id } }} passHref as='Edit'

            >
              <MuiLink onClick={() => handleEditClick(params.row.id)} sx={{ cursor: 'pointer' }}>{params.row.id}</MuiLink>
            </NextLink>
          } else {
            return params.row.id
          }
        }
      },
      {
        field: 'customerNm', headerName: '顧客名', type: 'string', headerAlign: 'center', align: 'left', flex: 0.8,
        renderCell: (params) => {
          if (!params.row.datFlg) {
            return <NextLink
              href={{
                pathname: "BasicRegist",
                query: { customerCd: params.row.customerCd }
              }} passHref as="BasicRegist"
            >
              <MuiLink>{params.row.customerNm}</MuiLink>
            </NextLink>
          } else {
            return params.row.customerNm
          }
        }
      },
      { field: 'inboundAtFormat', headerName: '入電日時', type: 'date', headerAlign: 'center', flex: 0.8 },
      { field: 'requestAtFromTo', headerName: '希望日時	', type: 'date', headerAlign: 'center', flex: 0.8 },
      {
        field: 'statusNm', headerName: '状態', type: 'string', headerAlign: 'center', flex: 0.5,
        renderCell: (params) => {
          if (!params.row.datFlg) {
            return <NextLink
              //  href='/'
              href={{ pathname: 'Detail', query: { statusNm: params.row.statusNm } }} passHref as='Detail'

            >
              <MuiLink onClick={() => handleDetailClick(params.row.statusNm)} sx={{ cursor: 'pointer' }}>{params.row.statusNm}</MuiLink>
            </NextLink>
          } else {
            return params.row.statusNm
          }
        }
      },
      { field: 'respondCnt', headerName: '追客回数', type: 'string', headerAlign: 'center', flex: 0.8 },
      { field: 'memo', headerName: 'メモ', type: 'string', headerAlign: 'center', flex: 1 },
      { field: 'fdInfo', headerName: 'FD情報', type: 'string', headerAlign: 'center', flex: 0.7 },
      { field: 'telNo', headerName: '電話番号', type: 'number', headerAlign: 'center', flex: 1 },
      { field: 'latestAt', headerName: '最終対応日時', type: 'string', headerAlign: 'center', flex: 0.9 },
      { field: 'customerCd', headerName: '顧客コード', type: 'string', headerAlign: 'center', hide: true },
      { field: 'returnRespondId', headerName: '折り返し発信ID', type: 'string', headerAlign: 'center', hide: true },
      { field: 'typeDiv', headerName: '折り返し種別', type: 'string', headerAlign: 'center', hide: true },
      { field: 'productCd', headerName: 'プロダクトコード', type: 'string', headerAlign: 'center', hide: true },
      { field: 'datFlg', headerName: '論理削除フラグ', type: 'string', headerAlign: 'center', hide: true },
      { field: 'urgentFlg	', headerName: '至急フラグ', type: 'string', headerAlign: 'center', hide: true },
      { field: 'statusDiv', headerName: '状態', type: 'string', headerAlign: 'center', hide: true },
      { field: 'callFlg', headerName: '電話可否フラグ', type: 'string', headerAlign: 'center', hide: true },
    ],
    [handleEditClick, handleDetailClick]
  );

  const rows = [
    {
      id: '更新', customerNm: 'スナッチテスト', inboundAtFormat: '2022/12/1', requestAtFromTo: '2021/11/1',
      statusNm: '未着手', respondCnt: '0022244', memo: 'スナッチテスト', fdInfo: '2021/11/1',
      telNo: '0704477834', latestAt: '2022/12/1', customerCd: '2021/11/1',
      returnRespondId: '2022/12/1', typeDiv: '2021/11/1', productCd: '2022/12/1', datFlg: false,
      urgentFlg: '2021/11/1', statusDiv: '2022/9/1', callFlg: '2021/1/1'
    },
    {
      id: '更新', customerNm: 'テスト太郎', inboundAtFormat: '2022/12/1', requestAtFromTo: '2021/11/1',
      statusNm: '未着手', respondCnt: '003333', memo: 'お名前：テスト太郎', fdInfo: '2021/11/1',
      telNo: '0704477834', latestAt: '2022/12/1', customerCd: '2021/11/1',
      returnRespondId: '2022/12/1', typeDiv: '2021/11/1', productCd: '2022/12/1', datFlg: false,
      urgentFlg: '2021/11/1', statusDiv: '2022/9/1', callFlg: '2021/1/1'
    },
    // {
    //   id: '更新', customerNm: 'テスト', inboundAtFormat: '2022/12/1', requestAtFromTo: '2021/11/1',
    //   statusNm: '未着手', respondCnt: 'スナッチテスト', memo: 'お名前：テスト太郎', fdInfo: '2021/11/1',
    //   telNo: '0704477834', latestAt: '2022/12/1', customerCd: '2021/11/1',
    //   returnRespondId: '2022/12/1', typeDiv: '2021/11/1', productCd: '2022/12/1', datFlg: false,
    //   urgentFlg: '2021/11/1', statusDiv: '2022/11/1', callFlg: '2021/4/1'
    // },

  ];
  console.log(rows)
  return (

    <Box
      sx={{
        height: '100%',
        width: '100%',
      }}>
      <div  >
        <DataGrid
          style={{ height: "400px", width: "100%" }}
          sx={StyledDataGrid.grid}
          // getRowId = {(row) => row.id}
          // rows={rows}
          rows={rows.map((row) => {
            return row
          })}
          getRowHeight={() => 'auto'}
          columns={columns}
          pageSize={pageSize}
          onPageSizeChange={(newPageSize) => setPageSize(newPageSize)}
          rowsPerPageOptions={[10, 15, 30, 50]}
          pagination
          getRowClassName={(params) => {
            if (params.row.callResultFlg == true) {
              return `backGroundColorCallFlg--${params.row.callResultFlg}`
            } else if (params.row.orderStatus == '00009') {   // 保留伝票
              return `backGroundColorOrderStatus--${params.row.orderStatus}`
            } else if (params.row.callClassId == '00001') {
              return `backGroundColorCallClassId--${params.row.callClassId}`
            } else if (params.row.callClassId == '00002') {
              return `backGroundColorCallClassId--${params.row.callClassId}`
            } else if (params.row.callClassId == '00003') {
              return `backGroundColorCallClassId--${params.row.callClassId}`
            } else if (params.row.callClassId == '00004') {
              return `backGroundColorCallClassId--${params.row.callClassId}`
            } else if (params.row.callClassId == '00006') {
              return `backGroundColorCallClassId--${params.row.callClassId}`
            } else if (params.row.callClassId == '00008') {
              return `backGroundColorCallClassId--${params.row.callClassId}`
            } else if (params.row.callClassId == '00011') {
              return `backGroundColorCallClassId--${params.row.callClassId}`
            } else if (params.row.callClassId == '00013') {
              return `backGroundColorCallClassId--${params.row.callClassId}`
            } else if (params.row.callClassId == '00014') {
              return `backGroundColorCallClassId--${params.row.callClassId}`
            } else {
              return `backGroundColorOther--Other`
            }
          }}
          localeText={jaJP.components.MuiDataGrid.defaultProps.localeText}
          disableColumnMenu                     // グリッド内のカラムメニュー無効
          disableSelectionOnClick               // 行選択無効
          density='compact'
        />
        {confirmConfig && <ConfirmDiallog {...confirmConfig} />}
      </div>
    </Box >

  );
}

export default DataGrid_list;
\\\\\\\\\\\\\\\\\
detail
------------------

import React, { useState, useContext } from 'react'
import * as Layout from "../Layout/index";
import Head from 'next/head';
import { DataGrid, GridColDef, jaJP } from '@mui/x-data-grid';
import { Button, Grid, Link as MuiLink } from '@mui/material';
import Box from '@mui/material/Box';
import NextLink from "next/link";
import { StyledDataGrid, StyledHeader, StyledTitleHeader } from "../../styles/Styled";
import Table from '@mui/material/Table';
import TableBody from '@mui/material/TableBody';
import TableCell from '@mui/material/TableCell';
import TableContainer from '@mui/material/TableContainer';
import TableHead from '@mui/material/TableHead';
import Paper from '@mui/material/Paper';
import { useTheme } from "@mui/material/styles";
import { useRouter } from 'next/router'
import { makeStyles } from '@mui/styles';
import { DialogDispContext } from '../../pages/CallCenter/CallBack/index';
import { styled } from '@mui/material/styles';


const Root = styled('div')(({ theme }) => ({
    padding: theme.spacing(1),
    [theme.breakpoints.down('md')]: {
        boxDetail: {
            display: "flex",
            Width: "50%",
        }

    },
    [theme.breakpoints.up('md')]: {

    },
    [theme.breakpoints.up('lg')]: {

    },
}));

const useStyles = makeStyles({
    tableContainer: {
        maxWidth: "100%",
        justifyContent: "center",
    },
    boxDetail: {
        height: '100%',
        width: '100%',
        maxWidth: "50%",
        marginTop: "30px"
    },
});

const commonStyles = {
    bgcolor: 'background.paper',
    m: 1,
    borderColor: 'text.primary',
    width: '5rem',
    height: '5rem',

};
interface rows {
    id: number,
    name: string;
    content: string;
}
const rows = [
    {
        id: 1,
        label: "対応日時",
        name: "2022/05/17 14:13"
    },
    {
        id: 2,
        label: "状態",
        name: "未着手"
    },
    {
        id: 3,
        label: "対応担当者",
        name: "電話研修2"
    },
    {
        id: 4,
        label: "備考",
        name: "楽しむお名前：テスト太郎"
    },
    {
        id: 4,
        label: "備考",
        name: "楽しむお名前：テスト太郎"
    },
    {
        id: 4,
        label: "備考",
        name: "楽しむお名前：テスト太郎"
    },
    {
        id: 4,
        label: "備考",
        name: "楽しむお名前：テスト太郎"
    },
    {
        id: 4,
        label: "備考",
        name: "楽しむお名前：テスト太郎"
    },
    {
        id: 3,
        label: "対応担当者",
        name: "電話研修2"
    },
    {
        id: 4,
        label: "備考",
        name: "楽しむお名前：テスト太郎"
    },
    {
        id: 4,
        label: "備考",
        name: "楽しむお名前：テスト太郎"
    },
    {
        id: 4,
        label: "備考",
        name: "楽しむお名前：テスト太郎"
    },
    {
        id: 4,
        label: "備考",
        name: "楽しむお名前：テスト太郎"
    },
    {
        id: 4,
        label: "備考",
        name: "楽しむお名前：テスト太郎"
    },

    {
        id: 4,
        label: "備考",
        name: "楽しむお名前：テスト太郎"
    },
    {
        id: 4,
        label: "備考",
        name: "楽しむお名前：テスト太郎"
    },
    {
        id: 4,
        label: "備考",
        name: "楽しむお名前：テスト太郎"
    },
    {
        id: 4,
        label: "備考",
        name: "楽しむお名前：テスト太郎"
    },
    {
        id: 4,
        label: "備考",
        name: "楽しむお名前：テスト太郎"
    },
    {
        id: 4,
        label: "備考",
        name: "楽しむお名前：テスト太郎"
    },


]

export default function Detail(props) {
    const theme = useTheme();
    const router = useRouter()
    const classes = useStyles();
    const { statusNm, tokCd, nowDate } = props;
    const [diaDispFlg, setDiaDispFlg] = useContext(DialogDispContext);
    const handleClose = () => {
        setDiaDispFlg(false);
    };
    return (
        <React.Fragment>
        {statusNm}
            <br />
            {tokCd}
            <br />
            {nowDate}
            <div>
                <div>
                    <Head>
                        <title>折り返し対応履歴</title>
                    </Head>
                    <div>
                        折り返し対応履歴
                    </div>
                    {/* <br /> */}
                    <Root>
                        <Box
                            className={classes.boxDetail}>
                            <Grid>
                                {/* <Paper elevation={3}>    */}
                                <Box sx={{
                                    height: '100%',
                                    width: '100%',
                                    '& > :not(style)': { width: '135ch' },
                                }}
                                >
                                    <Grid container spacing={4} >
                                        <Grid item xs={5.05} md={4.9} sm={4.9} lg={5.55}>
                                            <Box
                                                display="flex"
                                                justifyContent="flex-end"
                                                alignItems="flex-end"
                                            >
                                                <Button color="primary" size='large' variant="contained"
                                                    type="button" onClick={() => router.push('/CallCenter/CallBack/Edit')}
                                                >更新
                                                </Button>
                                            </Box >
                                        </Grid>
                                        <Grid item xs={5}>
                                            <Button color="primary" size='large' variant="contained"

                                                type="button" onClick={() => router.push('/CallCenter/CallBack')}
                                            >戻る
                                            </Button>
                                        </Grid>
                                    </Grid>
                                </Box >
                                <br />
                                <Box
                                    sx={{
                                        bgcolor: 'info.main',
                                        color: 'background.paper',
                                        p: 2,
                                        textAlign: 'center',
                                    }}
                                >
                                    <StyledTitleHeader>
                                        <strong>折り返し対応履歴</strong>
                                    </StyledTitleHeader>
                                </Box>
                                <Paper elevation={3} sx={{
                                    maxWidth: "100%", width: '100%',
                                }}>
                                    <div style={{
                                        overflowX: 'auto', height: 'calc(520px - 35px)'
                                    }}>
                                        <TableContainer component={Paper}
                                            className={classes.tableContainer}>

                                            <Table sx={{ maxWidth: "100%", commonStyles, boder: 0.5 }}  >
                                                {rows.map((item, index) => (
                                                    <TableHead key={index}  >
                                                        <TableCell sx={{
                                                            fontWeight: "bold",
                                                            commonStyles,
                                                            border: 0.5
                                                        }} align="right">
                                                            {item.label}
                                                        </TableCell>
                                                        <TableCell sx={{ commonStyles, border: 0.5 }} align="left">
                                                            {item.name}
                                                        </TableCell>
                                                    </TableHead>
                                                ))}
                                            </Table>
                                        </TableContainer >
                                    </div>
                                </Paper>
                            </Grid>
                        </Box >
                    </Root>
                </div>
            </div >
        </React.Fragment >
    )
};

----edit
-import React, { useRef, useState, useContext} from "react";
import Head from 'next/head';
import * as Layout from "../Layout/index";
import FormControlLabel from '@mui/material/FormControlLabel';
import { Box, Grid, FormControl, FormLabel, Button, FormGroup, Checkbox, Autocomplete, TextareaAutosize, Paper } from "@mui/material";

import { InputSelect } from "../Atoms/Select/Select";
import TextField from '@mui/material/TextField';
import { AdapterDateFns } from '@mui/x-date-pickers/AdapterDateFns';
import { LocalizationProvider } from '@mui/x-date-pickers/LocalizationProvider';
import { DesktopDatePicker } from '@mui/x-date-pickers/DesktopDatePicker';
import { useOperatorList } from "../../features/Operator/Selector";
import { StyledHeader, StyledTitleHeader } from "../../styles/Styled";
import ja from 'date-fns/locale/ja'
import { styled } from '@mui/material/styles';
import { DialogDispContext } from '../../pages/CallCenter/CallBack/index';


const Root = styled('div')(({ theme }) => ({
    padding: theme.spacing(1),
    [theme.breakpoints.down('md')]: {

    },
    [theme.breakpoints.up('md')]: {

    },
    [theme.breakpoints.up('lg')]: {

    },
}));
// ヘッダ部エリアのスタイル設定
//  （検索条件のエリア）
export const StyleEdit = styled("div")(({ theme }) => ({
    padding: 0,
    marginLeft: "20px",
    fontSize: 24,
}));
// import dayjs, { Dayjs } from 'dayjs';
import { useRouter } from 'next/router'
export default function Edit(props){

    const { id, tokCd, nowDate } = props;
    const [diaDetailFlg, setDiaDetailFlg] = useState(false);
    const [diaEditFlg, setDiaEditFlg] = useContext(DialogDispContext);
    const [diaDispFlg, setDiaDispFlg] = useContext(DialogDispContext);
    const handleShowDetail = (props) => {
        console.log(props);
        setDiaEditFlg(false);
        setDiaDetailFlg(true);
      };
    const handleClose = () => {
        setDiaDispFlg(false);
    };
    const router = useRouter()
    const [callNb, setCallNb] = useState();
    const dispDayValidPattern = "^[0-9/]+$";
    const dispDayRef = useRef<HTMLInputElement>(null);
    const [dispDayError, setDispDayError] = useState(false);

    const handleChangeCallNb = (e) => {
        const value = e.target.value.replace(/\D/g, "");
        setCallNb(value);
    };
    // const  = setday for to- come();
    const [yearMonthDay, setYearMonthDay] = useState<{ yearMonthDaySt: Date; yearMonthDayEd: Date }>({
        yearMonthDaySt: new Date(),
        yearMonthDayEd: new Date(),
    })


    // ************************
    // バリデーションチェック
    // ************************
    const formValidation = (): boolean => {
        let valid = true;

        // 移動日
        const valDay = dispDayRef?.current;
        if (valDay) {
            const ok = valDay.validity.valid;
            setDispDayError(!ok);
            valid &&= ok;
        }

        return valid;
    };

    const handleChangeYearMonthDay = (newValue: Date | null, fieldYearMonthDay: string) => {
        setYearMonthDay(prev => ({ ...prev, [fieldYearMonthDay]: newValue }))
    };

    const operatorList = useOperatorList();

    // 担当者セレクトボックス
    const handleOnChangeOperator = (key: string, column: string, value: string | number) => {
        const promise = async () => {
        };
        promise();
    };

    const handleOnChangeHani = (key: string, column: string, value: string | number) => {
        const promise = async () => {
        };
        promise();
    };
   
    return (
        <React.Fragment>
            <Head>
                <title>折り返し状態更新</title>
            </Head>
            <div>
            {id}
            <br />
            {tokCd}
            <br />
            {nowDate}
                <br />
                <br />
                <Root>
                    <div style={{ width: "100%" }}>
                        <Paper elevation={3} sx={{
                            maxWidth: 680,
                            // mt: -1.5,
                        }}>
                            <Box
                                sx={{
                                    bgcolor: 'info.main',
                                    color: 'background.paper',
                                    p: 2,
                                    textAlign: 'center',
                                }}
                            >
                                <StyledTitleHeader>
                                    <strong>折り返し状態更新</strong>
                                </StyledTitleHeader>
                            </Box>
                            <br />
                            <Box sx={{ flexGrow: 1 }}>
                                <Box sx={{
                                    height: '100%',
                                    width: '100%',
                                    '& > :not(style)': { width: '80ch' },
                                }}
                                    component="form"
                                    noValidate
                                    autoComplete="off"
                                >
                                    {/* 状態を表示する */}
                                    <Grid container spacing={2} alignItems='left' justifyContent='left'>
                                        <Grid item xs={2.8} >
                                            <StyleEdit>
                                                <InputSelect
                                                    list={operatorList}
                                                    title="状態"
                                                    name="operatorNm"
                                                    indx="operatorNm"
                                                    value="operatorId"
                                                    column="operatorNm"
                                                    unqkey="operatorId"
                                                    defaultValue={''}
                                                    onChange={handleOnChangeOperator}
                                                    options={{ blank: false, all: false }}
                                                />
                                            </StyleEdit>
                                        </Grid>
                                        <Grid item xs={4} sx={{ marginTop: "4px" }}  >
                                            <StyledHeader>
                                                <FormGroup aria-label="position" row >
                                                    <FormControlLabel
                                                        control={<Checkbox />}
                                                        label="スナッチ規定数対応"
                                                    />
                                                </FormGroup>
                                            </StyledHeader>
                                        </Grid>
                                    </Grid>
                                </Box >
                                {/* <br /> */}
                                <Box
                                    component="form"
                                    noValidate
                                    autoComplete="off"
                                    sx={{
                                        height: '100%',
                                        width: '100%',
                                        '& > :not(style)': { width: '80ch' },
                                    }}>
                                    <StyleEdit>
                                        <Grid container spacing={4} sx={{ paddingTop: "9px", }} >
                                            <Grid item xs={3}     >
                                                {/* <StyleEdit> */}
                                                <LocalizationProvider
                                                    dateAdapter={AdapterDateFns} adapterLocale={ja}>
                                                    <DesktopDatePicker
                                                        className='StyledBookYearMonthDay'
                                                        inputFormat='yyyy/MM/dd'
                                                        mask='____/__/__'
                                                        value={yearMonthDay.yearMonthDaySt}
                                                        onChange={(newValue) => handleChangeYearMonthDay(newValue, "yearMonthDaySt")}
                                                        renderInput={(params) =>
                                                            <TextField
                                                                {...params}
                                                                required
                                                                id="dispDay"
                                                                label="希望日時（yyyy/mm/dd）"
                                                                variant="filled"
                                                                size="small"
                                                                sx={{ width: 220 }}
                                                            />
                                                        }
                                                    />
                                                </LocalizationProvider>
                                                {/* </StyleEdit> */}
                                            </Grid>
                                            <Grid item xs={1} sx={{ marginTop: "10px" }} >
                                                {/* <StyleEdit> */}
                                                <InputSelect
                                                    list={operatorList}
                                                    title="時"
                                                    name="operatorNm"
                                                    indx="operatorNm"
                                                    value="operatorId"
                                                    column="operatorNm"
                                                    unqkey="operatorId"
                                                    defaultValue={''}
                                                    onChange={handleOnChangeOperator}
                                                    options={{ blank: false, all: false }}
                                                />
                                                {/* </StyleEdit> */}
                                            </Grid>
                                            <Grid item xs={0.9} sx={{ marginTop: "10px" }} >
                                                {/* <StyleEdit> */}
                                                <InputSelect
                                                    list={operatorList}
                                                    title="分"
                                                    name="operatorNm"
                                                    indx="operatorNm"
                                                    value="operatorId"
                                                    column="operatorNm"
                                                    unqkey="operatorId"
                                                    defaultValue={''}
                                                    onChange={handleOnChangeOperator}
                                                    options={{ blank: false, all: false }}
                                                />
                                                {/* </StyleEdit> */}
                                            </Grid>
                                            <Grid item xs={3} >
                                                <StyleEdit>
                                                    <FormGroup row sx={{ marginTop: "22px" }}>
                                                        <FormControlLabel
                                                            value="top"
                                                            control={<Checkbox />}
                                                            label="至急"
                                                        // labelPlacement="top"
                                                        />
                                                    </FormGroup>
                                                </StyleEdit>
                                            </Grid>
                                        </Grid>
                                    </StyleEdit>
                                </Box>
                                {/* <br /> */}
                                <Box sx={{
                                    height: '100%',
                                    width: '100%',
                                    '& > :not(style)': { width: '80ch' },
                                }}>  <StyleEdit>
                                        {/* 希望日時を表示する */}
                                        <Grid container spacing={4} sx={{ paddingTop: "9px" }} >
                                            <Grid item xs={3}>
                                                {/* <StyleEdit> */}
                                                <LocalizationProvider dateAdapter={AdapterDateFns} adapterLocale={ja}>
                                                    <DesktopDatePicker
                                                        className='StyledBookYearMonthDay'
                                                        inputFormat='yyyy/MM/dd'
                                                        mask='____/__/__'
                                                        value={yearMonthDay.yearMonthDayEd}
                                                        onChange={(newValue) => handleChangeYearMonthDay(newValue, "yearMonthDayEd")}
                                                        renderInput={(params) =>
                                                            <TextField
                                                                {...params}

                                                                required
                                                                id="dispDay"
                                                                label="希望日時（yyyy/mm/dd）"
                                                                variant="filled"
                                                                size="small"
                                                                sx={{ width: 220 }}
                                                            />
                                                        }
                                                    />
                                                </LocalizationProvider>
                                                {/* </StyleEdit> */}
                                            </Grid>
                                            <Grid item xs={1} sx={{ marginTop: "10px" }} >
                                                {/* <StyleEdit> */}
                                                <InputSelect
                                                    list={operatorList}
                                                    title="時"
                                                    name="operatorNm"
                                                    indx="operatorNm"
                                                    value="operatorId"
                                                    column="operatorNm"
                                                    unqkey="operatorId"
                                                    defaultValue={''}
                                                    onChange={handleOnChangeOperator}
                                                    options={{ blank: false, all: false }}
                                                />
                                                {/* </StyleEdit> */}
                                            </Grid>
                                            <Grid item xs={1.9} sx={{ marginTop: "10px" }} >
                                                {/* <StyleEdit> */}
                                                <InputSelect
                                                    list={operatorList}
                                                    title="分"
                                                    name="operatorNm"
                                                    indx="operatorNm"
                                                    value="operatorId"
                                                    column="operatorNm"
                                                    unqkey="operatorId"
                                                    defaultValue={''}
                                                    onChange={handleOnChangeOperator}
                                                    options={{ blank: false, all: false }}
                                                />
                                                {/* </StyleEdit> */}
                                            </Grid>
                                        </Grid>
                                    </StyleEdit>
                                </Box >
                                {/* <br /> */}
                                <Box sx={{
                                    height: '100%',
                                    width: '100%',
                                    '& > :not(style)': { width: '80ch' },
                                    paddingTop: "17px"
                                }}>
                                    {/* 電話番号を表示する */}
                                    <Grid container spacing={4} >
                                        <Grid item xs={3}>
                                            <StyleEdit>
                                                <TextField
                                                    InputProps={{
                                                        disableUnderline: true,
                                                    }}
                                                    required
                                                    value={callNb}
                                                    onChange={handleChangeCallNb}
                                                    error={callNb === ""}
                                                    helperText={callNb === "" ? '電話番号に入力してください。' : ' '}
                                                    id="callNumber"
                                                    label="電話番号"
                                                    variant="filled"
                                                    // defaultValue="07044778740"
                                                    // onChange={(newValue) => setNumber(newValue)}
                                                    size="small"
                                                    sx={{ width: 220 }}

                                                />
                                            </StyleEdit>
                                        </Grid>
                                    </Grid>
                                </Box >
                                <Box sx={{
                                    height: '100%',
                                    width: '100%',
                                }}>
                                    {/* 電話番号を表示する */}
                                    <Grid container spacing={1} >
                                        <Grid item xs={3}>
                                            <StyleEdit>
                                                <TextField
                                                    InputProps={{
                                                        disableUnderline: true,
                                                    }}
                                                    maxRows={4}

                                                    multiline
                                                    rows={4}
                                                    id="callNumber"
                                                    label="メモ"
                                                    variant="filled"
                                                    size="small"
                                                    sx={{ width: 573 }}
                                                />
                                            </StyleEdit>
                                        </Grid>
                                    </Grid>
                                </Box >
                                <br />
                                <Box sx={{
                                    height: '100%',
                                    width: '100%',
                                }}>
                                    {/*ボタン */}
                                    <StyledHeader>
                                        <Grid container spacing={4} >
                                            <Grid item xs={8.7} lg={8.7} sm={8.7} md={8.7}>
                                                <Box
                                                    display="flex"
                                                    justifyContent="flex-end"
                                                    alignItems="flex-end"
                                                >
                                                    <Button color="primary" size='large' variant="contained"
                                                        type="button" onClick={() =>handleShowDetail(id)}
                                                    >更新
                                                    </Button>
                                                </Box >
                                            </Grid>
                                            <Grid item xs={2} >
                                                <Button color="primary" size='large' variant="contained"
                                                    type="button" onClick={() => router.push('/CallCenter/CallBack')}
                                                >戻る
                                                </Button>
                                            </Grid>
                                        </Grid>
                                    </StyledHeader>
                                </Box >
                                <br />
                            </Box>
                        </Paper>
                    </div>
                </Root>
            </div>
        </React.Fragment >
    )
};

// export const Index = (): JSX.Element => {
//     return (
//         <React.Fragment>
//             <Layout.Index mainComponent={<Main />} title='折り返し状態更新' />
//         </React.Fragment>
//     );
// };

// export default Edit;

-----------------index callback
---------------
import React, { useRef, useState, createContext } from "react";
import Head from 'next/head';
import * as Layout from "../../../components/Layout/index";
import Radio from '@mui/material/Radio';
import RadioGroup from '@mui/material/RadioGroup';
import FormControlLabel from '@mui/material/FormControlLabel';
import { Box, Grid, FormControl, MenuItem, Select, BoxProps, SelectChangeEvent, FormLabel, Button } from "@mui/material";
import KeyboardArrowDownIcon from '@mui/icons-material/KeyboardArrowDown';
import { InputSelect } from "../../../components/Atoms/Select/Select";
import TextField from '@mui/material/TextField';
import { AdapterDateFns } from '@mui/x-date-pickers/AdapterDateFns';
import { LocalizationProvider } from '@mui/x-date-pickers/LocalizationProvider';
import { DesktopDatePicker } from '@mui/x-date-pickers/DesktopDatePicker';
import { useOperatorList } from "../../../features/Operator/Selector";
import * as store from '../../../stores/Store';

import { useCallClassList } from "../../../features/CallClass/Selector";
import { StyledHeader, StyledTitleHeader } from "../../../styles/Styled";
import { useDispatch } from "react-redux";
import DataGrid_list from "../../../components/CallBack/DataGrid"
import * as commonTips from "../../../constants/CommonTips";
import ja from 'date-fns/locale/ja'
import TipsIcon from "../../../components/Common/Tips";
import { styled } from '@mui/material/styles';
import { useCompleteClassList } from "../../../features/CompleteClass/Selector";
import { useCallBackConditions, useCallBackList } from "../../../features/CallBack/Selector";
import { useTodayReceptionConditions, useTodayReceptionList } from "../../../features/TodayReception/Selector";
import { fetchAsynccallBackSliceList } from "../../../features/CallBack/Operation";

import { fetchAsyncCompleteClassList } from "../../../features/CompleteClass/Operation";
import callBackSlice from "../../../features/CallBack/Slice";

import Edit from '../../../components/CallBack/Edit'
import Detail from '../../../components/CallBack/Detail'


export const StyleCallBack = styled("div")(({ theme }) => ({
  paddingLeft: theme.spacing(1),
  paddingTop: '10px',
  fontSize: 24,
}));
export const StyleCallBackDiv = styled("div")(({ theme }) => ({
  paddingLeft: theme.spacing(1),
  paddingTop: '5px',
  fontSize: 24,
}));

// ヘッダ部エリアのスタイル設定
//  （検索条件のエリア）
export const DialogDispContext = createContext(null);

export const Main = (): JSX.Element => {

  // const msgList = useMsgList();
  const [value, setValue] = useState("total");

  const operatorList = useOperatorList();

  function handleChangeStatus(e) {
    setValue(e.target.value);
    console.log(e.target.value);
  }

  // 担当者セレクトボックス
  const handleOnChangeOperator = (key: string, column: string, value: string | number) => {
    const promise = async () => {
    };
    promise();
  };

  const handleOnChangeHani = (key: string, column: string, value: string | number) => {
    const promise = async () => {
    };
    promise();
  };

  const [bookYearMonthDay, setBookYearMonthDay] = React.useState<String | null>(
    String(new Date()),
  );
  const handleBookYearMonthDayChange = (newValue: String | null) => {
    setBookYearMonthDay(newValue);
  };


  const [diaEditFlg, setDiaEditFlg] = useState(false);
  // 本日の予定（月単位）コンテキスト
  const [diaDetailFlg, setDiaDetailFlg] = useState(false);


  return (
    <React.Fragment>
      <Head>
        <title>折返し</title>
      </Head>
      <DialogDispContext.Provider value={[diaEditFlg, setDiaEditFlg,diaDetailFlg,
      setDiaDetailFlg
      ]}>
        {
          diaEditFlg && !diaDetailFlg ?
            <div>
              <Edit
                // id={rows.find.name}
                tokCd='3000'
                nowDate='2020'
              />
            </div>
            : !diaEditFlg && diaDetailFlg ?
              <div>
                <br />
                <br />
                <Detail
                />
              </div>
              :
              <div >
                <div >
                  <br></br>
                  <br />
                  <Grid container alignItems='right' justifyContent='right'>
                    <TipsIcon
                      title={commonTips.CST_TIPS_TITLE_CALLBACK}
                      content={commonTips.CST_TIPS_CONTENT_CALLBACK}
                    >
                    </TipsIcon>
                  </Grid>
                  <Box sx={{ flexGrow: 1 }}>
                    <Box sx={{
                      height: '100%',
                      // marginBottom : 1,
                      width: '100%',
                      '& > :not(style)': { width: '140ch' },

                    }}>
                      <Grid container spacing={2} alignItems='left' justifyContent='left' >
                        <Grid item xs={3} sm={3} md={3} style={{
                          padding: "10px 0 10px 16px",
                        }}>
                          <FormLabel sx={{ fontSize: "0.8rem" }}>
                            状態
                          </FormLabel>
                          <RadioGroup

                            row
                            onChange={handleChangeStatus}
                          >
                            <FormControlLabel value="total"
                              checked={value === 'total'}
                              control={<Radio />} label="全て" />
                            <FormControlLabel value="started "

                              control={<Radio />} label="未着手" />
                            <FormControlLabel value="hold"

                              control={<Radio />} label="保留" />
                          </RadioGroup>
                        </Grid>
                        <Grid item xs={4} >
                          <StyleCallBack>
                            <InputSelect
                              list={operatorList}
                              title="プロダクト"
                              name="operatorNm"
                              indx="operatorNm"
                              value="operatorId"
                              column="operatorNm"
                              unqkey="operatorId"
                              defaultValue={''}
                              onChange={handleOnChangeOperator}
                              options={{ blank: false, all: false }}
                            />
                          </StyleCallBack>
                        </Grid>
                      </Grid>
                    </Box >
                    {/* <br /> */}
                    <Box sx={{
                      height: '100%',
                      width: '100%',
                      '& > :not(style)': { width: '140ch' },
                    }}>
                      <Grid container spacing={2} >
                        <Grid item xs={3}>
                          <LocalizationProvider dateAdapter={AdapterDateFns} adapterLocale={ja}>
                            <DesktopDatePicker
                              className='StyledBookYearMonthDay'
                              inputFormat='yyyy/MM/dd'
                              mask='____/__/__'
                              value={bookYearMonthDay}
                              onChange={handleBookYearMonthDayChange}
                              renderInput={(params) =>
                                <TextField
                                  style={{ margin: "0" }}
                                  {...params}
                                  required
                                  id="dispDay"
                                  label="完了表示	（yyyy/mm/dd）"
                                  variant="filled"
                                  size="small"
                                  sx={{ width: 237 }}
                                />
                              }
                            />
                          </LocalizationProvider>
                        </Grid>
                        <Grid item xs={2}>
                          <StyleCallBackDiv>
                            <InputSelect
                              list={operatorList}
                              title="折り返し種別"
                              name="operatorNm"
                              indx="operatorNm"
                              value="operatorId"
                              column="operatorNm"
                              unqkey="operatorId"
                              defaultValue={''}
                              onChange={handleOnChangeOperator}
                              options={{ blank: false, all: false }}
                            />
                          </StyleCallBackDiv>
                        </Grid>
                        <Grid item xs={4} >
                          <StyleCallBackDiv>
                            <Button color="primary" size='large' variant="contained"

                            >再表示</Button>
                          </StyleCallBackDiv>
                        </Grid>
                      </Grid>
                    </Box>
                    {/* <br /> */}
                    <br />
                    <Box
                      sx={{
                        bgcolor: 'info.main',
                        color: 'background.paper',
                        p: 2,
                        textAlign: 'center',
                        maxWidth: "100%",
                      }}
                    >
                      <StyledTitleHeader>
                        <strong>折り返し一覧</strong>
                      </StyledTitleHeader>
                    </Box>
                    <Box sx={{
                      height: '80%',
                      width: '100%',
                    }}>
                      <DataGrid_list />
                    </Box>
                  </Box>
                </div>
              </div>
        }
      </DialogDispContext.Provider>
    </React.Fragment>
  )
};

export const Index = (): JSX.Element => {
  return (
    <React.Fragment>
      <Layout.Index mainComponent={<Main />} title='折り返し' />
    </React.Fragment>

  );
};

export default Index;
