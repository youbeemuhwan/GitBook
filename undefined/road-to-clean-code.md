# Road To Clean Code

Before

```
private void extractFiles(List<MultipartFile> files, Board saveBoard) throws IOException {
        for(MultipartFile file : files){
            if(!(file.getContentType().equals("image/jpeg") || file.getContentType().equals("image/png"))){
                throw new RuntimeException("해당 첨부파일 형식이 올바르지 않습니다.");
            }

            String originalFilename = file.getOriginalFilename();
            String saveFileName = createSaveFileName(originalFilename);
            file.transferTo(new File(getFullPath(saveFileName)));

            BoardImageRequestDto  boardImageRequestDto = BoardImageRequestDto.builder()
                    .uploadImageName(originalFilename)
                    .storeImageName(saveFileName)
                    .board(saveBoard)
                    .build();

        BoardImage boardImageEntity = boardImageRequestDto.toEntity(boardImageRequestDto);
        boardImageRepository.save(boardImageEntity);


        }
    }
```



After

```
private BoardImageRequestDto extractReviewImageFilesToDto(List<MultipartFile> files, Board saveBoard) throws IOException {
        BoardImageRequestDto boardImageRequestDto = new BoardImageRequestDto();
        
        for(MultipartFile file : files){
            if(!(file.getContentType().equals("image/jpeg") || file.getContentType().equals("image/png"))){
                throw new RuntimeException("해당 첨부파일 형식이 올바르지 않습니다.");
            }

            String originalFilename = file.getOriginalFilename();
            String saveFileName = createSaveFileName(originalFilename);
            file.transferTo(new File(getFullPath(saveFileName)));

           boardImageRequestDto.builder()
                    .uploadImageName(originalFilename)
                    .storeImageName(saveFileName)
                    .board(saveBoard)
                    .build();
        }
        
        return boardImageRequestDto;
        
    }

    private void uploadReviewImageByDto(BoardImageRequestDto boardImageRequestDto){
        BoardImage boardImageEntity = boardImageRequestDto.toEntity(boardImageRequestDto);
        boardImageRepository.save(boardImageEntity);
    }
```



### Modification <a href="#modification" id="modification"></a>



* 메서드 이름

기존의 의미를 알아보기 힘든 메서드 명 대신 해당 메서드의 성격을 더 명확하게 보여줄수있는 이름으로 수정하였다.



* 책임 분할

단순히 extractFiles 메서드 안에 이미지 파일을 가져와서 가공하는 로직, 가공한 파일을 Dto에 담는 로직, 해당 dto를 엔티티로 만들어 Repository 에 저장하는 로직 까지 너무 많은 책임들이 담겨있었다.

그래서 파일 가공 & 해당 정보로 Dto 생성 을 묶고 엔티티화 해서 저장하는 로직은 따로 메서드화 해서 분리하였다.



